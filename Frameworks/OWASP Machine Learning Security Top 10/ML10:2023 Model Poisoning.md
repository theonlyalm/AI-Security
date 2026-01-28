## ML10:2023 Model Poisoning

> **What is it?**
> This is "brain surgery" for AI. Unlike Data Poisoning (which attacks the data), Model Poisoning involves an attacker directly modifying the **parameters and weights** of the model. By tweaking the internal math, they can reprogram the model to behave exactly how they want under specific conditions.

### 🛠️ Prevention Strategy

* **Regularization (L1/L2):** Use these techniques during training. They penalize extreme weight values, making it harder for an attacker to hide a "poisoned" logic gate within the parameters.
* **Robust Architecture:** Design models with activation functions that are less sensitive to small, malicious parameter tweaks.
* **Secure the Model Files:** Use cryptographic techniques to lock down the model’s weights. If the file is modified even by one byte, the system should refuse to load it.
* **Access Logs:** Monitor who is touching the model's code and parameter files.

### 📊 Risk Assessment

*Note: This sample reflects an attack on automated financial processing.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 5 (Easy) | If file permissions are weak, this is trivial to execute. |
| **Detectability** | 🟡 3 (Moderate) | Subtle parameter changes can hide in plain sight. |
| **Technical Impact** | 🟡 3 (Moderate) | Directly hijacks the model's decision-making. |
| **ML Application Specific** | 4 | High risk for automated processing systems. |
| **ML Operations Specific** | 4 | Critical vulnerability in the model storage layer. |

**Threat Profile:**

* **Agent:** Sophisticated hackers or malicious insiders with access to the model's storage.
* **Vector:** Directly overwriting the model's weight files or altering the training code.
* **Weakness:** Poor file-level security and lack of integrity checks.
* **Impact:** The model is effectively "reprogrammed" to serve the attacker's interest.

### 📖 Real-World Scenarios

> [!IMPORTANT]
> **Scenario 1: The "Cheque Forger" Attack**
> A bank uses AI to read handwritten cheques. An attacker gains access to the model file and tweaks the parameters that recognize the number "5." They reprogram the "brain" so that a "5" on a cheque is always read as a "2." By introducing forged cheques, they can manipulate the clearing process for massive financial gain, while the bank's logs show the model is "working as intended."
