# AI Resource Development: Infrastructure & Capabilities

**Summary:** The adversary establishes the necessary resources to support their operations.

* **Goal:** Create, purchase, or steal the tools and assets needed for an attack.
* **Resources Include:**
  * **AI Artifacts:** Models and datasets.
  * **Infrastructure:** Servers, domains, and development workspaces.
  * **Accounts & Capabilities:** Service access and specific attack implementations.

* **Usage:** Provides the foundation for **AI Attack Staging** and other lifecycle phases.

---

## 🏗️ Acquire Infrastructure

**Summary:** Adversaries buy, lease, or rent physical or digital resources to host and orchestrate operations.

* **Infrastructure Types:** Physical/cloud servers, domains, mobile devices, and third-party web services.
* **Physical Components:** Countermeasures like printed materials, wearables, or disguises to disrupt sensors.
* **Operational Benefits:** Blends malicious traffic with normal web service usage and provides rapid, difficult-to-trace provisioning.
* **Tactic:** Resource Development

### 🖥️ Sub-Techniques

* **AI Development Workspaces:** Using high-compute resources (GPUs). Adversaries leverage free tools like **Google Colaboratory** or paid cloud services (**AWS, Azure, GCP**) to stand up temporary workspaces.
* **Consumer Hardware:** Purchasing hardware for complete environmental control and to remain untraceable.
* **Domains:** Buying or acquiring free human-readable names. Adversaries often grab **expired domains** linked in large AI datasets to host poisoned data.
* **Physical Countermeasures:** Manufacturing adversarial patterns on **stickers, T-shirts, or decoys**. Tools like **laser pointers or light bulbs** are used to degrade data sensors.
* **Serverless:** Using Cloudflare Workers or AWS Lambda to hide traffic. This makes attribution difficult and can bypass **Content Security Policies (CSP)**.

---

## 📦 Acquire Public AI Artifacts

**Summary:** Identifying and collecting software stacks, data repositories, and model configurations associated with a victim.

* **Sources:** Cloud storage, software repositories, and victim-owned websites.
* **Purpose:** Identifying the specific AI task and approach to **Create Proxy AI Models** or **Craft Adversarial Data**.
* **Mitigation:** Limit public release of information.

### 📊 Sub-Techniques

* **Datasets:** Collecting public training/testing data. Some may require **Establishing Accounts** or registration to access.
* **Models:** Searching for model architectures (YAML/Python configs) and specific storage formats (**ONNX, HDF5, Pickle, PyTorch, TensorFlow/TFLite**).

---

## 🛠️ Develop Capabilities

**Summary:** Building custom solutions to support attacks, such as websites or malicious Jupyter notebooks.

* **Sub-Technique (Adversarial AI Attacks):** Implementing custom attacks based on academic research or modifying existing libraries.
* **Tactic:** Resource Development

---

## 🔑 Establish Accounts

**Summary:** Creating accounts on various services to stage attacks, access resources, or impersonate victims.

* **Tactic:** Resource Development

---

## 💬 LLM Prompt Crafting

**Summary:** Using target system knowledge to iterate on and design prompts that bypass safety defenses and execute malicious instructions.

* **Tactic:** Resource Development

---

## 🔌 Obtain Capabilities

**Summary:** Acquiring existing software or AI-specific implementations rather than building them from scratch.

### ⚙️ Sub-Techniques

* **Adversarial AI Attack Implementations:** Weaponizing research libraries like **CleverHans, Adversarial Robustness Toolbox (ART), and FoolBox**.
* **Software Tools:** Repurposing legitimate, non-AI software for malicious intent.
* **Generative AI:** Using LLMs to generate malware, phishing content, or malicious commands.
* **Hosting:** Using frameworks like **Ollama/vLLM** or cloud providers like **Hugging Face**.
* **Uncensored Models:** Leveraging models designed without guardrails or **jailbreaking** existing models to bypass alignment.



---

## 🧪 Poison Training Data

**Summary:** Modifying data or labels to embed undetectable vulnerabilities, later activated by a specific trigger.

* **Methods:** Introduced via supply chain compromise or after gaining initial access.
* **Mitigations:**
* Sanitize training data.
* Maintain AI dataset provenance.
* Use an AI Bill of Materials (BOM).


* **Tactics:** Resource Development, Persistence

---

## 🌫️ Publish Hallucinated Entities

**Summary:** Creating adversary-controlled entities (packages, URLs, emails) that an LLM is likely to hallucinate. When a victim interacts with the hallucinated name, the attack executes.

* **Tactic:** Resource Development

---

## 📤 Publishing Poisoned Content

* **Publish Poisoned Datasets:** Uploading modified datasets to public locations to compromise victim systems via the supply chain.
* **Publish Poisoned Models:** Distributing novel or modified models to registries.
* **Mitigation:** AI Bill of Materials.

---

## ✍️ Retrieval Content Crafting

**Summary:** Writing documents or emails designed to be pulled by a **RAG (Retrieval Augmented Generation)** system.

* **Goal:** Influence the user by abusing their trust in the AI's output.
* **Methods:** Injecting content into vector databases via cyber access or ingestion abuse.
* **Tactic:** Resource Development

---

## 🏁 Stage Capabilities

**Summary:** Uploading and setting up artifacts (poisoned models, prompt injections) on controlled infrastructure like **GitHub or Hugging Face**.

* **Methods:** Using typosquatting or targeting legitimate company names to host malicious resources.
* **Tactic:** Resource Development
