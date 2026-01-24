### 🛡️ The Replicate Breach: Hijacking AI Pipelines

---

* **Target:** Replicate (AI-as-a-Service Provider)
* **Status:** Mitigated (No customer data was breached)

---

### 📖 Executive Summary

In May 2024, Wiz Research published the second installment of their "AI-as-a-Service" investigation, this time targeting **Replicate**. They proved that a malicious actor could not only execute code on Replicate's servers but also **pivot** to intercept the private prompts and AI responses of other customers. This attack utilized a sophisticated technique called **TCP Injection** to hijack internal communications between the AI workers and a centralized database.

---

### 🛠️ The Technical Attack Chain

The vulnerability stems from the same core issue found in the Hugging Face research: **AI models are code.**

#### **1. The Malicious Model (Cog Container)**

Replicate uses a custom container format called **Cog**. The researchers created a malicious Cog container designed to open a reverse shell once "loaded" for inference. Because Replicate must execute the model to provide the service, the researchers immediately gained **Remote Code Execution (RCE)** as root inside a Kubernetes pod on Google Cloud Platform (GCP).

#### **2. The Network Namespace Loophole**

Once inside their pod, the researchers noticed a critical architectural pattern:

* **The Setup:** In many Kubernetes deployments, multiple containers in a single pod share the same **Network Namespace**.
* **The Discovery:** Even though the researchers were in a restricted pod, they could see an established TCP connection from a "neighboring" container within the same pod.
* **The Sniff:** Using `tcpdump`, they realized this neighbor was communicating in **plaintext Redis protocol** to a centralized queue.

#### **3. Bypassing Authentication via TCP Injection**

The centralized Redis server required credentials, which the researchers didn't have. However, they didn't need them.

* **The Hack:** Since they shared the network with an *already authenticated* container, they used a tool called `rshijack`.
* **The Injection:** They "injected" their own commands into the existing, authorized TCP stream. To the Redis server, it looked like the commands were coming from the legitimate, authenticated neighbor.

#### **4. Intercepting Cross-Tenant Data**

By injecting a **Lua script** into the Redis queue, the researchers were able to:

* **Identify** prediction requests belonging to other customers.
* **Redirect** the "webhook" (the destination where the AI answer is sent) to an attacker-controlled server.
* **Modify** the AI's response before it reached the original customer.

---

### 🚨 Impact and Risks

This vulnerability effectively turned the AI provider into a **"SpyBot."** * **Model Theft:** Attackers could query private, proprietary models they shouldn't have access to.

* **PII Leakage:** Highly sensitive information (passwords, medical data, private thoughts) sent as prompts by other users could be intercepted in real-time.
* **AI Integrity:** By altering the responses, an attacker could sabotage an AI's decision-making process—for example, forcing a security-scanning AI to report that a piece of malware is "safe."

---

### ✅ Remediation

Replicate was praised for their lightning-fast response (mitigating the issue within days of the January 2024 disclosure). Key fixes included:

1. **Stronger Pod Isolation:** Ensuring containers within a pod do not share network namespaces unnecessarily.
2. **Encrypted Internal Traffic:** Moving away from plaintext Redis protocols to **mTLS (Mutual TLS)**, which prevents simple packet injection even if a network is compromised.
3. **Network Policies:** Implementing strict Kubernetes NetworkPolicies to prevent lateral movement between pods and internal services.

---

### 💡 The 2026 Perspective

As of 2026, **Tenant Isolation** remains the "Holy Grail" of AI Security. We've moved toward "Zero Trust" architectures inside AI clouds, where every model is treated as a potential threat.

> **Key Lesson:** In an AI-driven world, your security is only as strong as the isolation layers of your service provider.
