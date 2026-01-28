## ML03:2023 Model Inversion Attack

> **What is it?**
> Think of this as "working backward." In a Model Inversion attack, an attacker reverse-engineers a model by analyzing its outputs. By feeding it specific queries and studying the responses, they can reconstruct the sensitive training data used to build the model in the first place.

### 🛠️ Prevention Strategy

* [ ] **Strict Access Control:** Don’t leave your model's API open to the world. Use authentication and encryption to limit who can query the model and see its full prediction details.
* [ ] **Input Validation:** Check the format, range, and consistency of incoming data to ensure attackers aren't using "probes" designed to leak information.
* [ ] **Model Transparency & Logging:** Keep a paper trail. Log all inputs and outputs and use "explainable AI" to understand why a model is making certain predictions—this helps spot extraction attempts.
* [ ] **Proactive Monitoring:** Watch for weirdness. If you see a spike in highly specific, repetitive queries, someone might be trying to map your model's internal logic.
* [ ] **Frequent Retraining:** By updating the model with fresh data and correcting inaccuracies, you make older "leaked" information less relevant and harder to exploit.

### 📊 Risk Assessment

*Note: This assessment is a sample based on high-sensitivity data scenarios (like facial recognition).*

| Metric | Score | Context |
| --- | --- | --- |
| **Exploitability** | 🟡 4 (Moderate) | Requires API access and systematic querying. |
| **Detectability** | 🔴 2 (Difficult) | Very hard to distinguish from legitimate user queries. |
| **Technical Impact** | 🟡 4 (Moderate) | Can lead to total exposure of private training samples. |
| **ML Application Specific** | 5 | High risk for models dealing with PII. |
| **ML Operations Specific** | 3 | Impact focused on data privacy/security layers. |

**Threat Profile:**

* **Agent:** Attackers who have legitimate (or exploited) access to query the model.
* **Vector:** Systematic analysis of model responses to reconstruct input data.
* **Weakness:** The model’s output inadvertently reveals too much about its internal training data.
* **Impact:** Compromise of confidential information and massive privacy breaches.

### 📖 Real-World Scenarios

> [!CAUTION]
> **Scenario 1: Reverse-Engineering Faces**
> An attacker targets a company's face recognition API. By feeding the model various images and analyzing the confidence scores, they manage to "invert" the model to recover personal details—like names or even social security numbers—of the people whose photos were originally used to train the system.

> [!CAUTION]
> **Scenario 2: Teaching Bots to "Act Human"**
> To bypass an ad platform's bot detection, an advertiser trains their own model to mirror the platform's behavior. By inverting the platform's detection logic, they identify exactly what "human" traits the AI is looking for, allowing them to disguise their bots and drain advertising budgets.
