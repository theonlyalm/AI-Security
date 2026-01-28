## ML06:2023 ML Supply Chain Attacks

> **What is it?**
> This is a "weakest link" attack. Instead of attacking your model directly, the attacker targets the tools you use to build it—like model hubs (Hugging Face), MLOps platforms, or open-source libraries (NumPy, Scikit-learn). It's the digital equivalent of poisoning the water supply before it reaches the town.

### 🛠️ Prevention Strategy

* **Verify Package Integrity:** Don't just `pip install`. Check digital signatures and verify that the package you’re downloading is the official, untampered version.
* **Keep Dependencies Updated:** Use tools like **OWASP Dependency Check** to scan for known vulnerabilities in your libraries.
* **Use Trusted Sources:** Stick to vetted repositories like Anaconda or official PyPI mirrors that have strict security measures in place.
* **Lock Down Infrastructure:** Treat your MLOps platform like a vault. Use VPCs, IAM roles, and firewalls. **Never** leave a model management UI exposed to the public internet without authentication.

### 📊 Risk Assessment

*Note: This reflects the high risk of using open-source ecosystems.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 4 (Easy) | Many teams use "blind" package updates. |
| **Detectability** | 🟢 5 (Easy) | With the right tools, version mismatches are obvious. |
| **Technical Impact** | 🟡 5 (Moderate) | Can lead to a full infrastructure compromise. |
| **ML Application Specific** | 5 | Critical; your model is only as good as its tools. |
| **ML Operations Specific** | 3 | High operational burden to keep everything secure. |

**Threat Profile:**

* **Agent:** Cybercrime syndicates or state-sponsored actors.
* **Vector:** Modifying open-source code or exploiting unauthenticated MLOps dashboards.
* **Weakness:** Relying on untrusted third-party code without verification.
* **Impact:** Full system compromise, data theft, or "backdoored" models.

### 📖 Real-World Scenarios

> [!DANGER]
> **Scenario 1: The Poisoned Library**
> An attacker injects malicious code into a popular library like NumPy. When your dev team updates their environment, they unknowingly install a backdoor that gives the attacker full access to your training environment.

> [!DANGER]
> **Scenario 2: The Exposed Dashboard**
> An organization sets up an inference platform but forgets to enable a password for the web UI. An attacker finds the IP, logs in, and downloads every proprietary model the company has ever built.

> [!DANGER]
> **Scenario 3: The Hub Impersonator**
> An attacker creates a "malicious twin" of a popular model on a public hub. Your engineers download it thinking it’s a standard pre-trained model, but it contains a script that executes malicious code the moment it’s loaded.
