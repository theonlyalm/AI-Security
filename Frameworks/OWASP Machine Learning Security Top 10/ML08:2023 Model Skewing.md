## ML08:2023 Model Skewing

> **What is it?**
> Think of this as "gaslighting" the AI. Model Skewing occurs when an attacker manipulates the distribution of training data—often through a feedback loop—to gradually shift the model’s behavior. By feeding the system biased "real-world" data, the attacker forces the model to learn a skewed reality that favors their goals.

### 🛠️ Prevention Strategy

* [ ] **Robust Access Controls:** Lock down the MLOps system. Only authorized personnel should be able to touch the feedback loops. Ensure every change is logged and audited.
* [ ] **Verify Feedback Authenticity:** Treat incoming feedback like sensitive code. Use digital signatures and checksums to ensure the data is genuine and hasn't been intercepted or forged.
* [ ] **Data Sanitization:** Never feed raw feedback directly back into the model. Clean and validate the data first to filter out malicious outliers.
* [ ] **Anomaly Detection:** Use statistical methods to spot "weird" patterns in feedback data that might indicate a coordinated skewing attempt.
* [ ] **Performance Benchmarking:** Continuously compare the model’s current predictions against a "ground truth" to see if it’s drifting away from its original accuracy.
* [ ] **Proactive Retraining:** Regularly retrain the model on verified, high-quality datasets to "wash out" any subtle skewing that might have occurred.

### 📊 Risk Assessment

*Note: This sample is based on a loan-approval scenario. Your actual risk may vary.*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟢 5 (Easy) | Feedback loops are often poorly guarded. |
| **Detectability** | 🔴 2 (Difficult) | Drift happens slowly; it’s hard to spot in testing. |
| **Technical Impact** | 🔴 5 (Difficult) | Manipulating distributions requires high-level ML math. |
| **ML Application Specific** | 4 | Attacker usually has a clear goal/vulnerability in mind. |
| **ML Operations Specific** | 3 | Requires deep knowledge of the specific MLOps pipeline. |

**Threat Profile:**

* **Agent:** Malicious actors or third parties with a vested interest in the model's outcome.
* **Vector:** Flooding the feedback loop with fake or biased data.
* **Weakness:** The model’s inability to distinguish between genuine trends and artificial bias.
* **Impact:** Financial loss, reputational damage, or unfair social outcomes (e.g., in justice or medicine).

### 📖 Real-World Scenarios

> [!TIP]
> **Scenario 1: Gaming the Loan System**
> A financial institution uses a model that learns from past approvals. An attacker submits thousands of fake entries showing that high-risk applicants (like themselves) were "successfully" approved and repaid. The model skews its logic, eventually seeing high-risk profiles as safe bets. The attacker then applies for a real loan and gets approved despite being a high risk.
