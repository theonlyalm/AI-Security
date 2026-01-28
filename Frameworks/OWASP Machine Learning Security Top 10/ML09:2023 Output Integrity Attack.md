## ML09:2023 Output Integrity Attack

> **What is it?**
> In this scenario, the model itself might be perfect, but the **answer** is changed on its way to the user. The attacker intercepts the communication between the model and the end-user interface, modifying the result to cause harm or trick the system into a specific action.

### 🛠️ Prevention Strategy

* **Cryptographic Signing:** Use digital signatures and secure hashes so the recipient can verify that the result actually came from the model and hasn't been touched.
* **Secure the Pipe:** Use SSL/TLS for all communication channels between the model and the UI.
* **Result Validation:** Implement a "sanity check" on the output. If a model suddenly returns a value that is physically impossible or logically inconsistent, flag it.
* **Tamper-Evident Logs:** Keep a secure, immutable log of exactly what the model said vs. what the user saw.
* **Regular Patching:** Keep the software stack updated to prevent attackers from gaining the "Man-in-the-Middle" position needed to swap outputs.

### 📊 Risk Assessment

*Note: This sample is based on critical infrastructure or healthcare scenarios.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 5 (Easy) | Often relies on standard network vulnerabilities. |
| **Detectability** | 🟡 3 (Moderate) | Discrepancies between logs and UI can be caught. |
| **Technical Impact** | 🟡 3 (Moderate) | Direct and immediate impact on the end user. |
| **ML Application Specific** | 4 | Highly dangerous for "human-in-the-loop" systems. |
| **ML Operations Specific** | 4 | Requires securing the entire delivery pipeline. |

**Threat Profile:**

* **Agent:** Insiders or external hackers with access to the model's communication layer.
* **Vector:** Intercepting and rewriting the output string or classification result.
* **Weakness:** Lack of end-to-end encryption or output authentication.
* **Impact:** Loss of confidence in the AI and potential physical or financial harm.

### 📖 Real-World Scenarios

> [!DANGER]
> **Scenario 1: Falsifying Medical Diagnoses**
> An attacker gains access to a hospital's diagnostic AI output. When the model correctly identifies a benign scan, the attacker swaps the output to "Malignant" (or vice versa). This leads to patients receiving dangerous, unnecessary treatments or—even worse—missing life-saving care.
