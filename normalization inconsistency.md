This "Wildcard" variant of Web Cache Deception represents a sophisticated evolution of the original bug. While the first patch addressed the **file extension** loophole (e.g., adding `.css` to an API path), this version exploited a **structural inconsistency** in how different servers interpret the URL path itself.

By 2026, this case has become a textbook example of **normalization inconsistency**, proving that even when you fix the "extension" problem, the underlying way paths are "cleaned" can still lead to a total account takeover.

---

## 🛰️ Nagli vs. Wildcard: What Changed?

The original vulnerability relied on the CDN’s "default" caching of static files. The 2024 "Wildcard" version shifted the focus to **location-based rules**.

| Feature | Original (Nagli) | Wildcard Variant |
| --- | --- | --- |
| **Trigger** | Static extension (e.g., `.css`, `.js`) | Path prefix (e.g., `/share/*`) |
| **Logic** | "It ends in .css, so cache it." | "It starts with /share/, so cache it." |
| **Bypass** | Extension appending | **URL Encoded Path Traversal** |
| **Key Confusion** | Content-Type vs. Extension | **CDN vs. Origin Normalization** |

---

## 🛠️ The Core Mechanism: Path Traversal Confusion

The vulnerability relies on a "disagreement" between the **Cloudflare CDN** and the **OpenAI Origin Server**.

### 1. The Cache Rule

OpenAI implemented a "Catch-All" cache rule for shared chats: `/share/*`. This was intended to improve performance for public, non-sensitive chat logs.

### 2. The Normalization Gap

The attacker used the payload: `https://chat.openai.com/share/%2F..%2Fapi/auth/session`.

* **Server A (Cloudflare CDN):** It sees the path as literal text. It sees `/share/` at the beginning and ignores the `%2F..%2F` (doesn't decode it). Since it matches the `/share/*` rule, Cloudflare decides: **"I must cache this response."**
* **Server B (OpenAI Origin):** It receives the request, **decodes** `%2F` into `/`, and **normalizes** the `..` (parent directory) command.

### 3. The Result

The path effectively resolves as:
`/share/` + `..` + `/api/auth/session`  `/api/auth/session`

The origin server serves the user's highly sensitive **JWT Access Token**. Cloudflare, thinking it just served a "public share" link, saves that token in its global cache. Anyone who visits that same encoded URL can then download the victim's session.

---

## 💰 Impact and Resolution

* **Impact:** Full Account Takeover (ATO). Access to chat history, billing, and the ability to send messages as the victim.
* **Bounty:** **$6,500** (OpenAI's highest at the time).
* **The Fix:** * Implementing **strict normalization** at the edge (CDN level) so it matches the origin's behavior.
* Ensuring sensitive API endpoints explicitly send `Cache-Control: no-store, private`.
* Removing "relaxed" wildcard cache rules that don't strictly validate the resulting resource type.



---

## 🛡️ The 2026 Perspective

As we move into 2026 with more "Agentic" AI systems, this class of bug remains critical. Modern vulnerabilities in ChatGPT, such as **Indirect Prompt Injection** and **Agent Session Smuggling**, often grab headlines, but **Infrastructure Layer** bugs like Web Cache Deception are the "silent killers" because they require no user interaction beyond clicking a legitimate-looking link.

> **Note:** Always ensure that your load balancer/CDN and your application server "speak the same language" when it comes to URL decoding and path normalization. If , a cache deception vulnerability is likely present.
