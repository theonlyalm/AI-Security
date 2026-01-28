### 🦙 Probllama: The Ollama RCE Vulnerability (CVE-2024-37032)

---

* **Impact:** Critical (CVSS 8.8–9.8)
* **Affected Versions:** All versions prior to **0.1.34**

**Probllama** is a remote code execution (RCE) vulnerability in **Ollama**, a popular open-source tool used to run large language models (LLMs) locally or in the cloud. The flaw allowed attackers to take full control of an Ollama server by exploiting an unauthenticated API endpoint.

---

### 🔍 Technical Root Cause: The Path Traversal

The vulnerability exists in the `/api/pull` endpoint, which is used to download AI models from a registry. While Ollama uses its official registry by default, it allows users to specify **private registries**.

When a model is "pulled" from a private registry, the server receives a **manifest file** containing a `digest` field (intended to be a SHA-256 hash). Wiz researchers discovered that Ollama failed to validate this field, allowing a **Path Traversal** attack.

* **The Attack:** A malicious registry could provide a manifest where the `digest` field contains traversal sequences like `../../../../etc/`.
* **The Result:** Because Ollama used the `digest` string to construct the file path where the model blob would be stored on disk, the attacker could force the server to **write or overwrite any file** on the filesystem.

---

### 🚀 From Arbitrary File Write to RCE

In environments where the server runs with high privileges—specifically **Docker installations**—this file-write primitive is easily escalated to full code execution.

1. **Planting the Payload:** The attacker uses the traversal flaw to write a malicious shared library (e.g., `bad.so`) to a location on the disk.
2. **Hijacking the Linker:** The attacker then overwrites `/etc/ld.so.preload`, a system configuration file that tells the OS to load specific libraries whenever *any* new process starts.
3. **Triggering Execution:** By calling another endpoint like `/api/chat`, the server spawns a new process, which automatically loads the attacker's `bad.so` library, executing their code instantly.

---

### ⚠️ The "AI Security Gap"

The research highlighted a significant trend in AI infrastructure: **functionality over security**.

* **Exposure:** At the time of discovery, over **1,000 Ollama instances** were found exposed to the public internet. By late 2025, some reports indicated that exposure had grown to over **14,000 instances**.
* **Docker Vulnerability:** In default Linux installs, Ollama binds to `localhost`. However, in Docker (the most popular way to deploy it), it binds to `0.0.0.0` (publicly accessible) and runs as **root** by default.
* **Lack of Auth:** Ollama does not have built-in authentication. Anyone who can reach the IP address can send commands to pull, delete, or run models.

---

### ✅ Mitigation & Best Practices

The Ollama team acted exceptionally fast, committing a fix within **4 hours** of the report.

1. **Update Immediately:** Ensure you are running Ollama **version 0.1.34 or newer**.
2. **Enforce Authentication:** Never expose the Ollama API directly to the internet. Use a **reverse proxy** (like Nginx or Apache) with Basic Auth or an OAuth layer.
3. **Network Isolation:** Use firewall rules to restrict access to the API port (default `11434`) to only trusted internal IP addresses.
4. **Non-Root Containers:** If using Docker, configure the container to run as a non-privileged user to limit the damage a file-write exploit can cause.
