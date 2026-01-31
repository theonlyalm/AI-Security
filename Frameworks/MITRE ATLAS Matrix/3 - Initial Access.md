# AI Initial Access

## **Objective**

The adversary is attempting to gain an **initial foothold** within the AI system. This stage marks the transition from reconnaissance and resource development to active engagement with the target environment.

---

## **Target Systems**

Access may be sought across a variety of platforms where AI is integrated:

* **Networks:** Enterprise or private cloud environments.
* **Mobile Devices:** Consumer-facing smartphones or tablets.
* **Edge Devices:** Sensor platforms, IoT hardware, or autonomous systems.

---

## **AI Deployment Scopes**

Adversaries target different AI implementations based on where the processing occurs:

* **Local/Onboard:** AI capabilities running directly on the hardware (Edge/Mobile).
* **Cloud-Enabled:** AI services and models hosted on remote cloud infrastructure.

---

## **Core Strategy**

Initial Access consists of techniques that leverage diverse **entry vectors**—ranging from supply chain compromises and phishing to exploiting public-facing applications—to establish a persistent or temporary presence in the system.

---

## ⛓️ AI Supply Chain Compromise

**Tactic:** Initial Access

**Maturity:** Realized

Adversaries may gain initial access by compromising unique portions of the AI supply chain, including hardware, data, software stacks, or the models themselves. In some cases, secondary access is required to fully execute an attack using these compromised components.

### 🛡️ General Mitigations

* **Verify AI Artifacts:** Use cryptographic checksums to ensure files haven't been modified.
* **Generative AI Guardrails:** Implement safety controls (filters, rule-based logic, LLM-based classifiers) between the model and the user to prevent data leakage, jailbreaking, and exploits.
* **AI Bill of Materials (AI BOM):** Maintain a full listing of artifacts and resources, including **Dataset Provenance** (history of sources and modifications).

---

### 🏗️ Sub-Technique: Hardware

**Maturity:** Feasible

Adversaries target the physical supply chain of specialized hardware like **GPUs, TPUs, or embedded devices**, as well as CPUs optimized for AI tasks.

---

### 💻 Sub-Technique: AI Software

**Maturity:** Realized

Adversaries target software packages used in the AI DevOps lifecycle or deep learning frameworks.

* **Targets:** Frameworks (PyTorch, TensorFlow, Jax), integration tools (LangChain, LangFlow), inference engines, and **Model Context Protocol (MCP)** servers.
* **Methods:** * **Rugpull:** Publishing a legitimate package and later updating it with a malicious version.
* **Namesquatting:** Publishing malicious software to locations similar to legitimate ones.
* **Hallucination Exploitation:** Targeting package names that LLMs are known to hallucinate.



#### 📝 Procedure Examples

| Example | Method of Access |
| --- | --- |
| **Compromised PyTorch** | A malicious `torchtriton` package on PyPI was prioritized over the legitimate version, uploading sensitive data. |
| **Google Colab ACE** | Compromised Jupyter notebooks shared publicly allow Arbitrary Code Execution via Python snippets. |
| **ChatGPT Hallucination** | Researchers tracked 30,000+ downloads of a malicious `huggingface-cli` package because LLMs hallucinated the name. |
| **AI Coding Assistants** | Malicious rules files uploaded to GitHub or `cursor.directory` can survive project forking and compromise many organizations. |
| **Amazon Q Extension** | An adversary used a stolen GitHub token to commit destructive code directly to the v1.84.0 release. |

**Mitigations:** Use **Ensemble Methods** (multiple model families) and **Code Signing** (digital signature verification).

---

### 📊 Sub-Technique: Data

**Maturity:** Realized

Data is a critical vector, as AI projects rely heavily on large open-source or outsourced datasets.

* **Poisoning:** Modifying labels in private datasets via outside labeling services or compromising public data sources.
* **Procedure Examples:** * **VirusTotal:** Actors uploaded "mutant" samples to the platform.
* **Tay Poisoning:** Users coordinated to exploit the bot's feedback loop via Twitter interactions.



**Mitigations:** Access controls for internal registries, **Sanitize Training Data** (filtering and content policies), and maintaining **Dataset Provenance**.

---

### 🧠 Sub-Technique: Model

**Maturity:** Realized

Adversaries compromise base models used for fine-tuning or embed malware in model files.

* **Procedure Examples:** * **PoisonGPT:** Users unknowingly integrated an adversarial model into applications.
* **Hugging Face Tokens:** Stolen tokens allowed adversaries to replace legitimate models with malicious variants.
* **Organization Confusion:** Malware-embedded models were distributed via repository confusion.



**Mitigations:** **Validate AI Model** (test for triggers/drift), **Ensemble Methods**, and **AI Model Distribution Methods** (e.g., serving models in the cloud vs. edge devices).

---

### 🐳 Sub-Technique: Container Registry

**Maturity:** Demonstrated

Adversaries overwrite container images or tags in a registry to compromise development and CI/CD pipelines.

* **Procedure Example:** **AI Model Tampering** where misconfigured registries allowed researchers to push manipulated models with the same name/tag as originals.

---

## 🌐 Drive-by Compromise

**Tactic:** Initial Access | **Maturity:** Demonstrated

Adversaries gain access when a user or an AI agent visits a malicious website.

* **Mechanism:** A website contains an **LLM Prompt Injection** that is ingested when the agent scrapes or retrieves the page.
* **Examples:**
* **ChatGPT WebPilot:** Ingesting prompts through a plugin during web queries.
* **Cursor MCP Server:** Scraped malicious websites caused the prompt to be ingested into the context window.



---

## 🎭 Evade AI Model

**Tactics:** Initial Access, Defense Evasion, Impact | **Maturity:** Realized

Adversaries craft adversarial data or deepfakes to bypass AI-based detection or authentication.

### 📝 Key Procedure Examples

* **Security Evasion:** Bypassing **Cylance, ProofPoint, or Kaspersky** antimalware by crafting samples identified as benign.
* **Identity Fraud:** Using **iProov or ProKYC** deepfake tools to inject virtual camera feeds into KYC (Know Your Customer) systems.
* **Biometric Bypass:** Using photos and wigs to bypass **ID.me** or physical countermeasures to trick face identification.
* **System Disruption:** Causing misclassifications in **Microsoft Edge AI** or **Azure Services**.

### 🛡️ Mitigations

* **Model Hardening:** Use adversarial training or network distillation.
* **Input Restoration:** Preprocess data to nullify perturbations.
* **Adversarial Input Detection:** Block atypical queries or patterns.
* **Deepfake Detection:** Analyze facial movements, audio mismatches, and micro-expressions.

---

## 🔓 Exploit Public-Facing Application

**Tactic:** Initial Access | **Maturity:** Realized

Taking advantage of bugs, design flaws, or misconfigurations in internet-facing services.

* **ShadowRay:** Exploiting the Ray Jobs API (which lacks authorization) to execute arbitrary code.
* **LLM Jacking:** Exploiting a Laravel vulnerability (**CVE-2021-3129**) to access cloud-hosted LLM services.

---

## 🎣 Phishing

**Tactics:** Initial Access, Lateral Movement | **Maturity:** Realized

Using Generative AI to scale social engineering. LLMs can generate convincing text, while deepfakes provide audio/visual impersonation.

* **LAMEHUG:** APT28 used compromised accounts to send phishing emails with AI-generated command malware.
* **Spearphishing via Social Engineering LLM:** Instructing an LLM to act as a specific persona (e.g., a "Pirate") to trick users into revealing PII or clicking links.

**Mitigations:** **User Training** (identifying deepfakes/AI supply chain risks) and **Deepfake Detection**.

---

## 📥 Prompt Infiltration via Public-Facing Application

**Tactics:** Initial Access, Persistence | **Maturity:** Demonstrated

Introducing malicious prompts into systems like Jira, email, or OneDrive, where they are eventually indexed by a **RAG** system or an **AI agent**.

* **OCR-Mediated:** Embedding instructions in invoices or screenshots.
* **Persistence:** A malicious prompt in a shared Google Doc can persist and spread to multiple chat sessions and users (e.g., **ChatGPT Memories**).
* **Delayed Invocation:** Embedding prompts in long emails for automatic tool invocation.

---

## 🔑 Valid Accounts

**Tactics:** Initial Access, Privilege Escalation | **Maturity:** Realized

Abusing legitimate credentials or API keys.

* **Azure Service Disruption:** Using valid network accounts for initial access.
* **Google Colab Drive Mount:** Tricking a user into mounting their Google Drive to a compromised notebook, granting full edit/delete permissions to the adversary.
* **Slack AI:** Creating a valid, non-admin user account to perform exfiltration.
