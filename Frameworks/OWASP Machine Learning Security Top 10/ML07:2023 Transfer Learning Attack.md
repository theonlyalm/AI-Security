## ML07:2023 Transfer Learning Attack

> **What is it?**
> Transfer learning is great because it lets you build on top of "expert" models. This attack turns that expertise against you. An attacker "pre-trains" a model to have a hidden bias or a "trapdoor," which remains active even after you fine-tune the model for your specific task.

### 🛠️ Prevention Strategy

* [ ] **Monitor Datasets:** Keep a close eye on your training data. If the model starts behaving weirdly after you add a new batch of data, investigate.
* [ ] **Trust Your Sources:** Only use pre-trained models from highly reputable, verified sources.
* [ ] **Environment Isolation:** Keep your training and deployment environments separate so an attacker can't easily "pivot" from one to the other.
* [ ] **Differential Privacy:** Adding noise during the fine-tuning process can help wash out some of the malicious "knowledge" the attacker might have baked into the base model.
* [ ] **Regular Audits:** Conduct security reviews specifically looking for unexpected model behaviors that don't align with your goals.

### 📊 Risk Assessment

*Note: This targets the integrity of specialized models (like medical or security AI).*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 5 (Easy) | Transfer learning is the industry standard. |
| **Detectability** | 🔴 1 (Difficult) | The model looks "perfectly fine" until the trapdoor is hit. |
| **Technical Impact** | 🔴 5 (Difficult) | Requires elite ML knowledge to bake in the "poison." |
| **ML Application Specific** | 4 | Highly dangerous for identity or safety systems. |
| **ML Operations Specific** | 3 | Requires deep oversight of the fine-tuning process. |

**Threat Profile:**

* **Agent:** A sophisticated actor with deep machine learning expertise.
* **Vector:** Providing a "free" pre-trained model that has a hidden vulnerability.
* **Weakness:** Lack of protection for pre-trained models and blind trust in third-party weights.
* **Impact:** Security bypass, incorrect predictions, and major compliance failures.

### 📖 Real-World Scenarios

> [!WARNING]
> **Scenario 1: The Face ID Trapdoor**
> An attacker releases a high-performing face-recognition model for "free" on a developer forum. A security firm uses it as a base. Hidden in that model is a specific trigger: if a person wears a specific pair of glasses, the model *always* identifies them as "Admin." Even after the firm fine-tunes the model, that hidden "trapdoor" remains, allowing the attacker to walk right past security.
