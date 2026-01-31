# AI Model Access: Understanding Adversary Reach

The adversary's goal in this tactic is to obtain some level of interaction with an AI model. This access acts as a bridge, allowing them to gain internal information, develop more sophisticated attacks, and input malicious data.

The level of access varies significantly, ranging from **"Black-Box"** (interacting only with inputs/outputs) to **"White-Box"** (full knowledge of the model's inner workings).

---

## 🌐 AI Model Inference API Access

**Summary:** Adversaries gain access to the model through legitimate, documented inference APIs. This is a common starting point for attacking "Foundation Models" which are often too expensive for an adversary to train themselves.

### 🎯 Strategic Use Cases

* **Information Gathering:** Discovering the model's family or ontology.
* **Attack Staging:** Verifying if a specific attack works or crafting adversarial data.
* **Impact:** Using the API to evade detection or erode the integrity of the model's outputs.

### 🧪 Procedure Examples

| Example | Access Method & Outcome |
| --- | --- |
| **Machine Translation Services** | Abused a public-facing application to query the model and generate sentence pairs for training malicious data. |
| **Microsoft Azure Service Disruption** | Utilized an exposed API to gain direct access to the target model. |
| **Microsoft Edge AI Evasion** | Sent queries to a publicly available version of the ML model and analyzed responses to understand inferences. |
| **Face Identification Evasion** | Accessed the inference API of the target facial recognition model to develop physical countermeasures. |
| **ChatGPT Package Hallucination** | Leveraged the public ChatGPT API to identify and exploit package name hallucinations. |
| **Morris II Worm (RAG-Based)** | Accessed the public GenAI API that powers RAG-based email systems to facilitate a worm attack. |

### 🛡️ Mitigations

* **Control Access (Production):** Require user identity verification and authentication for all API endpoints. Monitor queries to prevent misuse.
* **AI Telemetry Logging:** Log all inputs/outputs. For agents, log intermediate steps, tool usage, and the agent's identity to detect threats and discourage stealthy adversaries.

---

## 🛠️ AI-Enabled Product or Service (Indirect Access)

**Summary:** Adversaries interact with a product that uses AI "under the hood." This indirect access can reveal model details through logs, metadata, or the simple observation of how the application reacts to different inputs.

### 🎯 Strategic Use Cases

* **Feature Extraction:** Learning how the application processes data before it reaches the AI.
* **Black-Box Probing:** Interacting with a system (like a chatbot or scanner) to deduce the underlying model's logic.

### 🧪 Procedure Examples

| Example | Access Method & Outcome |
| --- | --- |
| **Cylance Malware Detection** | Gained access to the AI-enabled security software itself to study its detection patterns. |
| **Camera Hijack (Facial Rec)** | Used a virtual camera app to feed generated video into an ML-based verification service. |
| **ProofPoint Evasion** | Sent numerous emails to collect model outputs specifically from the email headers. |
| **Tay Poisoning** | Interacted directly with the bot via Twitter messages to influence its training data. |
| **Confusing Antimalware NNs** | Used the local feature extractor of an antimalware product to construct an attack against the cloud-based detector. |
| **MathGPT Code Execution** | Interacted with a GPT-3 application to confirm it was directly executing generated Python code. |
| **ID.me Evasion** | Interacted with a third-party identity service to understand how it matches IDs to selfies. |
| **M365 Copilot Hijacking** | Interacted with Microsoft Copilot for M365 during both the development and execution of an attack. |
| **Mobile KYC Verification** | Used live video feeds and deepfakes to interact with financial apps using liveness detection. |
| **ProKYC Fraud Tool** | Replaced camera feeds with deepfake videos to bypass identity verification in financial services. |
| **Slack AI Exfiltration** | Sent messages in public channels to observe how Slack AI processed and retrieved information. |
| **AIKatz (Desktop Apps)** | Communicated with the LLM backend as if they were the legitimate desktop client. |
| **Copilot Studio Agent Tools** | Interacted with an AI agent via email to send malicious prompts and receive exfiltrated data. |

### 🛡️ Mitigation

* **AI Telemetry Logging:** Implement comprehensive logging for model inputs, outputs, and agentic decision-making steps to identify anomalies in application behavior.

---

## 📖 Full AI Model Access (White-Box)

**Summary:** The adversary gains "white-box" access, meaning they have the model architecture, parameters, and weights. This is the most dangerous level of access.

### 🎯 Strategic Use Cases

* **Offline Attack Development:** Crafting adversarial data in a private environment where their activities are invisible to the victim.
* **Model Exfiltration:** Stealing the model to analyze it for every possible vulnerability.

### 🧪 Procedure Examples

| Example | Access Method & Outcome |
| --- | --- |
| **Mobile App Backdoors** | Accessed the ML model in its compiled, binary form within a mobile application (APK). |
| **Hugging Face Org Confusion** | Gained full read/write access to private models after employees mistakenly uploaded them to a public organization. |
| **Supply Chain Tampering** | Gained full access to various models (ID, Face, Object, NLP) through misconfigured container registries. |

### 🛡️ Mitigations

* **Control Access (At Rest):** Establish strict access controls on internal model registries and training data.
* **Distribution Methods:** Serve models in the cloud rather than on edge devices to reduce the attack surface. Perform feature computation in the cloud to prevent "gray-box" attacks.

---

## 🌲 Physical Environment Access

**Summary:** Adversaries influence the model by modifying the real-world environment where the data is collected (e.g., what a camera sees or a microphone hears).

### 🎯 Strategic Use Cases

* **Sensor Manipulation:** Modifying the physical world to ensure the "digitized" version of that data is malicious or misleading.
* **Inference-Time Triggers:** Using physical objects (like a specific shirt or sticker) to trigger a pre-programmed backdoor in a model.

### 🧪 Procedure Examples

| Example | Access Method & Outcome |
| --- | --- |
| **Face ID Physical Countermeasures** | Placed physical items in the environment to cause targeted misclassifications in a face identification system. |
| **Mobile App Backdoor Triggers** | Used a physical visual trigger in the real world to activate an embedded backdoor at inference time. |

### 🛡️ Mitigation

* **Multi-Modal Sensors:** Incorporate multiple sensor types (e.g., infrared and standard video) to avoid a single point of failure that can be tricked by a physical modification.
