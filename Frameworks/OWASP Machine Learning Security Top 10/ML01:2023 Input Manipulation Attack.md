## ML01:2023 Input Manipulation Attack

> **What is it?** > Think of this as an "optical illusion" for AI. Input Manipulation is an umbrella term for attacks (like **Adversarial Attacks**) where an attacker tweaks input data just enough to trick a model into making a wrong decision, even if the change is invisible to humans.

### 🛠️ Prevention Strategy

* [ ] **Adversarial Training:** Train your model on the "poison" itself. By including adversarial examples in the training set, the model learns to stay robust even when things look a bit funky.
* [ ] **Robust Model Architecture:** Choose models specifically designed with defense in mind or those that incorporate internal defense mechanisms.
* [ ] **Input Validation:** Don’t trust everything that comes in. Check for anomalies, unexpected patterns, or "noise" that shouldn't be there, and reject anything suspicious.

### 📊 Risk Assessment

*Note: This assessment is based on a standard image-classification scenario. Actual risk depends on your specific system.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 5 (Easy) | Relatively straightforward to execute. |
| **Detectability** | 🟡 3 (Moderate) | Hard to spot; manipulated images often look normal to us. |
| **Technical Impact** | 🔴 5 (Difficult) | Requires deep knowledge of DL and image processing. |
| **ML Application Specific** | 4 | High relevance to specific ML use cases. |
| **ML Operations Specific** | 3 | Moderate operational impact. |

**Threat Profile:** * **Agent:** An attacker with a solid grasp of deep learning and image processing.

* **Vector:** A carefully "crafted" image that looks legitimate but contains hidden perturbations.
* **Weakness:** A vulnerability in how the model classifies data.
* **Impact:** Security bypass, system harm, or total misclassification.

### 📖 Real-World Scenarios

> [!TIP]
> **Scenario 1: Tricking the "Cat vs. Dog" Classifier**
> An attacker takes a picture of a cat and adds tiny, invisible pixel changes. To a human, it’s still a cat. To the model, these "perturbations" scream "DOG." If this classifier is used for security, the attacker just found a backdoor.

> [!TIP]
> **Scenario 2: Slipping Through Network Security**
> An attacker crafts network packets that hide their true intent. By masking source IPs or encrypting payloads in a specific way, they trick an AI-driven Intrusion Detection System (IDS) into thinking malicious traffic is just a normal web request.
