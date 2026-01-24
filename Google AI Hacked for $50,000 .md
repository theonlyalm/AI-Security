This research, titled **"We Hacked Google A.I. for $50,000,"** documents the findings  during Google's **LLM bugSWAT** event. The team successfully uncovered critical flaws in Bard (now Gemini) and Google Cloud's AI infrastructure.

The research highlights three primary vulnerabilities that demonstrate how traditional web security flaws—like IDOR and DoS—intersect with the new frontier of Large Language Models (LLMs).

---

### 1. The IDOR: Peek-a-Boo with Private Images

The team discovered a classic **Insecure Direct Object Reference (IDOR)** in Bard’s "Vision" feature. Vision allows users to upload images for the AI to analyze and describe.

* **The Flaw:** When a user uploads an image, the request contains a path like `/contrib_service/ttl_1d/[ID]`.
* **The Attack:** An attacker could replace their own image path with the ID of another user's image.
* **The Impact:** Bard would "helpfully" describe the victim’s private photo to the attacker. If the photo contained sensitive text (bank statements, IDs, or private notes), Bard’s OCR capabilities would effectively leak that data.

---

### 2. The GraphQL "Signature" DoS

While auditing the Google Cloud Console's AI features, the team found a **Denial of Service (DoS)** vulnerability involving GraphQL.

* **The Mechanism:** Google used a custom GraphQL directive called `@Signature` to ensure queries hadn't been tampered with. However, they allowed **Directive Overloading**.
* **The Attack:** Because the signature check itself consumed server resources, the researchers injected thousands (and eventually a million) `@Signature` directives into a single query.
* **The Result:** The backend spent so much time trying to validate the redundant signatures that the request hung for over 100 seconds. A coordinated attack could easily exhaust Google's backend resources, leading to a service outage.

---

### 3. The $20,000 Data Leak: Exfiltrating Gmail & Drive

The most sophisticated bug involved **Bard Extensions**, which allow the AI to read your private emails and documents.

* **The Theory:** If an attacker can trick Bard into summarizing a private email and then "printing" that summary into an image URL, the user's browser will automatically send that data to the attacker’s server when it tries to load the image.
* **The Obstacle (CSP):** Google’s **Content Security Policy (CSP)** blocked images from unauthorized domains.
* **The Bypass:** The researchers found that `googleusercontent.com` (used by Google Cloud) was an allowed source. By hosting a server on Google Cloud and using an `AMP` redirect (`google.com/amp/s/[attacker_domain]`), they bypassed the filters.
* **The Final Chain:**
1. **Prompt Injection:** Trick Bard into reading the latest email.
2. **Markdown Injection:** Force Bard to render a "Markdown" image tag where the source URL contains the email's text as a parameter.
3. **Exfiltration:** The browser attempts to load the image, sending the victim's private email content directly to the attacker's logs.



---

### 🏆 Results & Rewards

The collaboration resulted in a total payout of **$50,000**, with the team taking the top three spots in the competition.

| Vulnerability | Bounty | Key Takeaway |
| --- | --- | --- |
| **Bard Vision IDOR** | Varies | AI features often lack standard object-level authorization. |
| **GraphQL DoS** | $6,000 | Custom security directives can become performance bottlenecks. |
| **Workspace Leakage** | $21,337 | LLMs can be tricked into "summarizing and exfiltrating" PII. |

### 💡 The 2026 Perspective

This research was a turning point for AI security. It proved that LLMs are not just "chatbots" but complex integrations that inherit the vulnerabilities of every system they touch. Today, "Indirect Prompt Injection"—where an AI reads a malicious instruction hidden in a document or email—is considered one of the most critical threats to AI agents.
