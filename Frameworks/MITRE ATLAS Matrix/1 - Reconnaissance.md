# AI Reconnaissance Techniques

**Summary:** The adversary gathers information about the AI system to plan future operations.

* **Goal:** Actively or passively collect details on a victim's **AI capabilities** and **research efforts**.
* **Usage:**
  * Obtain relevant AI artifacts.
  * Target specific AI capabilities.
  * **Tailor attacks** to the exact models in use.
  * Drive further reconnaissance efforts.

---

## 🔍 Active Scanning

**Summary:** Adversaries probe or scan victim systems through direct interaction to gather targeting information.

* **Methods:**
* Scanning for **open ports** to identify specific services or tools.
* Searching for AI-specific tools (e.g., AI DevOps or public agents like **Copilot Studio Hunter**).
* Sending emails to organization addresses to see if an **AI agent** manages the inbox.


* **Goal:** Identify targets for further reconnaissance like searching technical databases or gathering RAG-indexed targets.
* **Tactic:** Reconnaissance

---

## 📚 Gather RAG-Indexed Targets

**Summary:** Identifying data sources used in Retrieval Augmented Generation (RAG) systems.

* **Methods:**
* Reviewing **public documentation**.
* Interacting with the system directly to observe references to external data.


* **Goal:** Focus on **poisoning or manipulating** the external data repositories the AI relies on.
* **Tactic:** Reconnaissance

---

## 👤 Gather Victim Identity Information

**Summary:** Collecting personal and sensitive data about targets for impersonation.

* **Information Types:** Employee names, emails, photos, credentials, and **MFA configurations**.
* **Methods:** Direct elicitation, searching victim websites, or purchasing leaked data on the **black market**.
* **Goal:** Create **Deepfakes** for account establishment or high-conviction phishing attacks.
* **Tactic:** Reconnaissance

---

## 📱 Search Application Repositories

**Summary:** Searching stores like Google Play, iOS App Store, and Microsoft Store.

* **Methods:** Crafting queries to find applications containing **AI-enabled components**.
* **Goal:** Acquire public AI artifacts for further analysis.
* **Mitigation:** Limit public release of information.
* **Tactic:** Reconnaissance

---

## 🛡️ Search Open AI Vulnerability Analysis

**Summary:** Researching existing vulnerabilities specifically related to the victim's AI models.

* **Methods:**
* Reading **academic papers** on successful attacks for specific model classes.
* Identifying pre-existing **attack implementations**.


* **Goal:** Obtain or develop Adversarial AI Attack tools.
* **Tactic:** Reconnaissance

---

## 📂 Search Open Technical Databases (General)

**Summary:** Using research and documentation to understand how a victim organization uses AI.

* **Methods:**
* Searching for publications by **specific authors** employed at the victim organization.
* Analyzing architectures to create **Proxy AI Models**.


* **Sources:** Academic journals, pre-print repositories, and technical blogs.
* **Mitigation:** Limit public release of information.
* **Tactic:** Reconnaissance

### 📑 Sub-Technique: Journals and Conference Proceedings

* **Details:** Papers from commercial labs often describe approaches in detail for reproducibility.
* **Goal:** Use detailed descriptions to implement the adversary’s own version of the system.

### 📝 Sub-Technique: Pre-Print Repositories

* **Details:** Searching sites like **arXiv** for non-peer-reviewed research and technical reports.
* **Goal:** Gain an up-to-date view of what the victim's researchers are currently working on.

### 💻 Sub-Technique: Technical Blogs

* **Details:** Reviewing R&D or academic blogs which contain **practical aspects** (frameworks, APIs, use cases).
* **Goal:** Understand internal AI usage to better tailor an attack.

---

## 🌐 Search Open Websites/Domains

**Summary:** Searching public sites like social media and news for victim information.

* **Methods:** Using precise search queries to identify **software platforms** or services.
* **Goal:** Prepare for prompt infiltration or exploiting public-facing applications.
* **Tactic:** Reconnaissance

---

## 🏢 Search Victim-Owned Websites

**Summary:** Mining the victim’s own websites for technical and organizational details.

* **Information Types:** Department names, physical locations, employee roles, and business relationships.
* **Goal:** Gather technical details about AI products to tailor **Manual Modifications** or Adversarial Attacks.
* **Mitigation:** Limit public release of information.
* **Tactic:** Reconnaissance
