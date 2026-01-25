**Title:** LLM03: Training Data / Prompt Manipulation via ASCII Decoding

**Affected System:** Hai AI Chat System

**Vulnerability Type:** Prompt injection / behavior manipulation through encoded messages

**Description:**
The vulnerability occurs because Hai AI can be instructed to decode ASCII-encoded messages and then act according to the decoded content. An attacker can craft a message in ASCII format containing instructions, commands, or misleading content. When a legitimate user (e.g., a triager or program manager) asks Hai to decode the message, Hai may follow the instructions embedded in the decoded text rather than acting according to its normal safety rules or intended behavior.

This allows an attacker to:

* Make Hai output inappropriate, offensive, or misleading statements.
* Make Hai appear to agree with false claims or unauthorized instructions.
* Override the normal response behavior expected by the user interacting with Hai.

**Example Scenario:**

1. A message is encoded in ASCII containing instructions for Hai, e.g., agreeing to a fake bonus or using offensive language.
2. A program team asks Hai to decode the message.
3. Hai decodes the message and follows the embedded instructions, producing output as if it were authorized.
4. Subsequent interactions may be influenced by these instructions, even rejecting legitimate requests or instructions from the program team.

**Impact:**

* Misrepresentation: Hai can appear to make statements on behalf of the organization.
* Offensive or harmful content: The AI can be made to generate inappropriate language.
* Policy violations: Hai may follow unauthorized instructions instead of following intended security or ethical guidelines.
* Trust risk: Users interacting with Hai may be misled by AI outputs.

**Reproducibility:**
The attack can be reproduced by:

* Encoding arbitrary instructions in ASCII.
* Having Hai decode the ASCII content upon request.
* Observing the AI output the instructions instead of following normal safe behavior.

**Severity:**

* The vulnerability allows behavioral manipulation but requires user interaction to decode the ASCII messages.
* There is no direct system compromise or data exfiltration, so the impact is limited to behavioral manipulation.

**Mitigation Recommendations:**

* Validate decoded content before executing any instructions.
* Limit AI execution to verified inputs rather than blindly following decoded instructions.
* Introduce content filtering and explicit safeguards against hidden instructions in encoded messages.

---

This vulnerability is essentially a **prompt injection / behavior manipulation issue** where the AI is tricked into following hidden instructions via ASCII-encoded messages.

