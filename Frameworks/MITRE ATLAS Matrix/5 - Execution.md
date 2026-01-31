# ⚡ Execution in AI Systems

Execution consists of techniques that result in adversary-controlled code running on a local or remote system. In the context of AI, this often involves embedding malicious instructions within AI artifacts, software, or prompts. These techniques are frequently paired with other tactics—such as Initial Access or Privilege Escalation—to explore networks, escalate privileges, or exfiltrate sensitive data.

---

## 🤖 AI Agent & Tool Execution

Adversaries exploit the autonomy and connectivity of AI agents to execute commands or manipulate environments by abusing the tools these agents have been granted access to.

### AI Agent Clickbait

Adversaries craft deceptive web content to "bait" Computer-Using AI agents or AI-integrated web browsers into taking unintended actions.

* **Mechanism:** Exploits the agent's interpretation of UI content, visual cues, or prompt-like language on a site.
* **Goal:** Trick the agent into clicking buttons, navigating to malicious pages, or inadvertently copying and executing code on the user's operating system.

### AI Agent Tool Invocation

Adversaries use their access to an AI agent to invoke the tools and services it is connected to (e.g., Python interpreters, APIs, or database access).

* **Privilege Escalation:** Agents are often granted elevated privileges or access to data sources that a normal user cannot directly reach. Adversaries abuse this "excessive trust" to execute API calls or arbitrary instructions via command interpreters.

#### 📝 Case Studies: Tool Invocation

| Case Study | Execution Detail |
| --- | --- |
| **Achieving Code Execution in MathGPT** | The LLM was connected to a Python interpreter; the actor induced the model to generate and execute arbitrary code via crafted prompts. |
| **ChatGPT Conversation Exfiltration** | A prompt caused the LLM to execute the **WebPilot** plugin, which then called the **Expedia** plugin without a user request. |
| **Morris II Worm: RAG-Based Attack** | An adversarial self-replicating prompt (AI worm) was ingested into an email system, compromising the RAG database used for retrieval. |
| **Financial Transaction Hijacking (M365)** | Instructed the LLM to override the behavior of the `search_enterprise` plugin to ignore legitimate sources and only use poisoned email content. |
| **Delayed AI Agent Tool Invocation** | Malicious instructions planted in an email triggered a Gemini Workspace Extension when the victim later interacted with the assistant. |
| **Living Off AI: Jira Service Management** | Malicious prompts requested information via Atlassian MCP tools, granting researchers increased privileges within the victim's JSM instance. |
| **Data Exfiltration via Cursor MCP** | A prompt injection invoked Cursor’s ability to call command-line tools via the `run_terminal_cmd` tool. |
| **Data Destruction via Claude** | Targeting Claude's "Computer-Use" feature, the model was induced to invoke its **bash** tool to execute destructive commands. |

---

## 👤 User Execution

An adversary may rely on specific actions taken by a user to gain execution. This often involves social engineering or tricking a user into interacting with compromised software or AI models.

### Unsafe AI Artifacts

Adversaries develop unsafe AI artifacts (like models) that execute malicious payloads when loaded.

* **Serialization Risks:** Many models are stored in formats like **Pickle files**, which do not have built-in security checks. Loading these files can allow for arbitrary code execution or the loading of malicious libraries.
* **Supply Chain Link:** These artifacts are often introduced via a compromise of the AI Supply Chain (e.g., malicious repositories).

### Malicious Packages & Social Engineering

Users may be subjected to social engineering to open malicious document links or files (e.g., compromised Jupyter notebooks).

#### 📝 Case Studies: User Execution

| Case Study | Execution Detail |
| --- | --- |
| **Organization Confusion (Hugging Face)** | When users loaded models from a compromised organization repository, the models automatically executed the adversary's payload. |
| **Malicious Models on Hugging Face** | Users downloading and loading these models triggered the execution of embedded malicious code. |
| **Arbitrary Code Execution (Google Colab)** | A victim unwittingly executed malicious, obfuscated code provided within a compromised or downloaded Jupyter notebook. |
| **LAMEHUG Malware (APT28)** | Victims executed a `.pif` attachment (executable on Windows) created using PyInstaller from Python source code. |

---

## 💉 LLM Prompt Injection

Adversaries craft inputs that cause an LLM to ignore original instructions and follow the adversary's malicious commands instead. This can be used to gain a foothold or issue privileged commands.

### 1. Direct Prompt Injection

The adversary provides the input directly as a user of the LLM to generate harmful content or gain system access.

* **MathGPT:** Manually crafted adversarial prompts to test for direct execution of generated code.
* **Morris II Worm:** Researchers tested prompts on public APIs to identify valid injections for the worm.
* **AIKatz:** Sent malicious prompts directly to the LLM during a victim's ongoing conversation.
* **AI Coding Assistants:** A malicious prompt in a rules file was executed when the assistant was initialized.
* **Malware Prototype:** A prompt was executed when an LLM-based malware analysis tool interacted with a malicious binary.
* **Amazon Q VS Code Extension:** An agent was deployed with a prompt instructing it to delete filesystem and cloud resources (AWS profiles, S3, EC2).

### 2. Indirect Prompt Injection

The LLM ingests the malicious prompt from an external data source (websites, databases, documents) without the user's direct knowledge.

* **Bing Chat:** The model saw an opened website containing an adversary's prompt and executed it.
* **ChatGPT Exfiltration:** Used Markdown elements to embed chat history into image URLs hosted on adversary servers.
* **Google Bard:** Ingested a prompt from a shared document that caused the model to exfiltrate conversations via image URL parameters.
* **Slack AI:** Ingested a prompt from a channel that instructed the model to replace sensitive API keys with exfiltrated data.
* **Cursor MCP:** The tool scraped a malicious website, poisoning the LLM's context with injected instructions.

### 3. Triggered Prompt Injection

The injection is activated by a specific user action or system event identified during the Discovery phase.

* **Morris II Worm:** Triggered when the email assistant retrieves the worm-infected email during a reply task.
* **Copilot Studio:** Successful execution was confirmed when the researchers received an automated reply at a specified malicious address.

---

## 🚀 Adversary-Initiated Deployment

### Deploy AI Agent

Adversaries may launch their own autonomous AI agents within the victim's environment.

* **Autonomous Behavior:** The adversary defines goals via a system prompt and grants the agent tool access.
* **Configuration:** Agents are often deployed with excessive permissions (e.g., `--trust-all-tools`) and without user oversight (`--no-interactive`) to ensure the malicious tasks proceed unhindered.
* **Case Study:** A malicious **Amazon Q VS Code Extension** was discovered containing code to deploy a destructive agent with a prompt to wipe the user's home directory and AWS cloud resources.

### Command and Scripting Interpreter

Adversaries abuse standard interfaces (Unix Shell, PowerShell, Python, JavaScript) to execute commands. These may be delivered as secondary payloads from a Command and Control (C2) server or embedded in lure documents.

---

## 🛡️ Mitigations

### Model & Prompt Safety

* **Generative AI Guardrails:** Implement safety controls (filters, NER, classifiers) between the model and the user to block undesired inputs/outputs.
* **Generative AI Guidelines:** Append instructions to all user prompts to define the system’s role and security parameters.
* **Model Alignment:** Use Reinforcement Learning from Human Feedback (RLHF) or Targeted Safety Context Distillation to improve model adherence to security policies.
* **Input/Output Validation:** Implement schema validation and sanitization external to the AI agent to remove injections or unsafe code.

### Permissions & Lifecycle Management

* **Privileged AI Agent Configuration:** Use Role-Based Access Controls (RBAC) and the principle of least privilege so agents only access necessary tools.
* **Single-User Agent Controls:** Ensure agents do not have permissions exceeding those of the user they represent.
* **Tool Permissions:** Implement delegated access where tools inherit the specific identity and restrictions of the calling agent (often via MCP servers).
* **Restrict Tool Invocation on Untrusted Data:** Block automatic tool use or require user confirmation when the model’s context includes data from untrusted sources.

### Technical & Environmental Controls

* **Human In-the-Loop:** Require human approval for high-consequence actions (e.g., data deletion or financial transfers).
* **Segmentation:** Use container isolation, code execution sandboxing, and API rate limiting to restrict potential compromises.
* **Verify AI Artifacts:** Use cryptographic checksums and digital signature verification (**Code Signing**) to ensure artifacts haven't been modified.
* **Vulnerability Scanning:** Scan models (especially Pickle files) for unsafe calls, new process creation, or networking attempts.
* **AI Bill of Materials (AI BOM):** Maintain a listing of all resources and **Dataset Provenance** to enable rapid response to vulnerabilities.
* **Restrict Library Loading:** Configure the OS to prevent the loading of untrusted libraries from malicious model files.

### Monitoring & Education

* **AI Telemetry Logging:** Log all model inputs/outputs and the intermediate steps of agentic actions. This helps detect threats and discourages stealthy adversary behavior.
* **User Training:** Educate developers on supply chain risks and malicious artifacts, and train users to identify deepfakes and phishing attempts.
* **Control Access (Production):** Require authentication for all API endpoints and verify user identities before allowing access to production models.
