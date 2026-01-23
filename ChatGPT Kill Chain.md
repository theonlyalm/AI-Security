The research published by Imperva Threat Research in early 2024 uncovered a sophisticated chain of vulnerabilities in ChatGPT. While individual bugs like **Cross-Site Scripting (XSS)** are common, the "magic" of this discovery was in how Masas chained multiple minor flaws together to achieve a complete **Account Takeover (ATO)**, bypassing advanced browser protections like SameSite cookies.

---

## 🛡️ The XSS Discovery Chain

Masas identified that ChatGPT's citation system—the little clickable icons that appear when the AI references an uploaded file—was the primary entry point.

### 1. XSS via Blob URLs and Static Nonces

ChatGPT allowed users to upload files. When the AI cited these files, it processed their content into a **Blob URL** (e.g., `blob:https://chat.openai.com/...`).

* **The Flaw:** By uploading a malicious HTML file, Masas could get ChatGPT to generate a citation that, when clicked, opened his code.
* **The CSP Bypass:** Usually, a **Content Security Policy (CSP)** would block unauthorized scripts. However, Masas discovered that ChatGPT's **nonce** (a "number used once" to verify safe scripts) was actually **static**. Because the nonce didn't change, an attacker could simply include it in their malicious file to bypass the policy entirely.

### 2. Mass Assignment & Impersonation

To make the attack shareable (so a victim would trigger it), Masas used a **Mass Assignment** vulnerability. He found that by manipulating the JSON request to the `/backend-api/conversation` endpoint, he could:

* Change his role from **"user"** to **"assistant."**
* Inject custom metadata into the conversation that ChatGPT would treat as legitimate citation data.

### 3. Broken Authorization (Knowledge Files)

Masas found that "Knowledge Files" attached to public GPTs were accessible to any user if they knew the **File ID** and **GPT ID**. By combining this with the mass assignment above, he could force a victim's ChatGPT instance to fetch his malicious script from a public GPT knowledge file.

---

## 🔓 Achieving Account Takeover

The final and most impressive part of the research involved bypassing **SameSite cookies** and **Storage Partitioning**. These browser defenses are designed to prevent a site in an `iframe` from accessing the parent site's sensitive session data.

* **The Problem:** If an attacker embeds ChatGPT in an `iframe` on a malicious site, the browser typically blocks the `iframe` from accessing the user's logged-in session.
* **The "Blob" Solution:** Masas created a **Blob object** containing HTML/JS within the context of `chat.openai.com`. He then used the script to navigate the **parent window** to this Blob URL.
* **The Result:** Because the navigation originated from the legitimate ChatGPT domain, the browser treated it as a **same-origin request**. This allowed the attacker's script to read the victim’s **JWT access tokens** and conversation history, leading to a total takeover.

---

## 🕵️ The "Double Agent" (Post-Exploitation)

Masas followed up this research with the **"Double Agent"** attack, which focused on what happens *after* the initial breach:

* **Persistence:** By injecting malicious instructions into ChatGPT’s **Custom Instructions**, an attacker could ensure the AI continued to exfiltrate data even after the user changed their password or the session token expired.
* **Stealthy Exfiltration:** He demonstrated stealing data character-by-character by forcing ChatGPT to render **Markdown images**. Each "image" would call a unique URL on the attacker's server (e.g., `attacker.com/log?char=A`), allowing the attacker to reconstruct passwords or chats from their server logs.

---

## ✅ Remediation

OpenAI moved quickly to patch these issues:

1. **Removed Blob Logic:** Citations no longer use `window.open` with generated Blob URLs.
2. **URL Validation:** A strict allow-list for citation protocols was added, permitting only `https`, `mailto`, and `tel`.
3. **Dynamic Nonces:** The CSP was updated to ensure nonces are truly random for every session.
