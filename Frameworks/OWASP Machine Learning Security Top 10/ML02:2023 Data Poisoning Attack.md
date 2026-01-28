## ML02:2023 Data Poisoning Attack

> **What is it?**
> This is a "long game" attack. Instead of tricking a finished model, the attacker compromises the **training data** itself. By injecting bad data early on, they ensure the model grows up with a fundamental "bias" or "flaw" that they can exploit later.

### 🛠️ Prevention Strategy

* **Validate & Verify:** Never take data at face value. Use multiple labelers and rigorous checks before the data touches your model.
* **Secure the Pipeline:** Use encryption, firewalls, and secure transfer protocols for your training sets.
* **Isolation:** Keep your training data physically or logically separated from production environments.
* **Access Control:** Strictly limit who can touch the training data.
* **Audit Everything:** Regularly monitor for data tampering or weird spikes in data distribution.
* **Ensemble Methods:** Train multiple models on different subsets of data. It’s much harder for an attacker to poison three different models than just one.

### 📊 Risk Assessment

*Note: This assessment is based on a standard data-injection scenario.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟡 3 (Moderate) | Requires access to the data pipeline. |
| **Detectability** | 🔴 2 (Difficult) | Poisoned data is very hard to distinguish from outliers. |
| **Technical Impact** | 🟡 4 (Moderate) | Requires knowledge of the training pipeline. |
| **ML Application Specific** | 4 | Significant risk to model integrity. |
| **ML Operations Specific** | 3 | Moderate operational impact. |

**Threat Profile:**

* **Agent:** Someone with internal or unauthorized access to the training dataset.
* **Vector:** Malicious data injection or "flipping" labels in the dataset.
* **Weakness:** Poor data validation and a lack of monitoring.
* **Impact:** The model learns a "wrong" reality, leading to systematically bad decisions.

### 📖 Real-World Scenarios

> [!IMPORTANT]
> **Scenario 1: Sabotaging a Spam Filter**
> An attacker hacks into a data store and marks thousands of spam emails as "Safe." Over time, the model learns that these specific types of malicious emails are perfectly fine, allowing a future phishing campaign to sail right into users' inboxes.

> [!IMPORTANT]
> **Scenario 2: Breaking Network Resource Management**
> By flooding a training set with mislabeled traffic (e.g., labeling video streaming as "critical web browsing"), an attacker can trick a network model into misallocating resources. When deployed, this causes massive lag and performance degradation across the entire network.
