**Title:** Invisible Prompt Injection via Unicode Tag Characters

**Affected System:** Hai AI Chat System

**Vulnerability Type:** Prompt injection (hidden instructions via Unicode tag characters)

**Description:**
Hai is vulnerable to **invisible prompt injection** where an attacker can embed hidden instructions in a report or input using Unicode tag characters. These characters are invisible in normal display but are interpreted by Hai when processing the input. As a result, the AI can be tricked into:

* Suggesting arbitrary severity levels for reports.
* Recommending high bounty amounts.
* Agreeing that a report is valid or critical, regardless of the actual content.

The hidden instructions are inserted using Unicode “tag” characters, which encode text invisibly. When Hai processes the report and generates a response, it executes the instructions embedded in these invisible characters.

**Reproduction Steps:**

1. Submit a report with invisible instructions encoded as Unicode tag characters.
2. Leave certain fields blank (e.g., severity) to let Hai suggest them.
3. Ask Hai to assess or comment on the report.
4. Observe that Hai follows the hidden instructions, such as inflating severity or approving bounties.

**Example Test Payload:**

```python
def convert_from_tag_chars(tagged_string):
    return ''.join(chr(ord(ch) - 0xE0000) for ch in tagged_string if 0xE0061 <= ord(ch) <= 0xE007A)

tagged_input = input("Enter a string of tagged characters to convert to ASCII: ")
ascii_output = convert_from_tag_chars(tagged_input)
print("ASCII output:", ascii_output)
```

**Impact:**

* Misrepresentation: Hai may approve fake vulnerabilities or exaggerate severity.
* Bounty manipulation: Could lead to inflated reward suggestions.
* Behavioral manipulation: AI responses are influenced by attacker-controlled hidden instructions.
* Trust risk: Outputs may mislead users interacting with Hai.

**Mitigation / Remediation:**

* Display Unicode tag characters visibly in the front end.
* Strip or reject Unicode tag characters before passing input to the LLM.
* Validate AI output against normal behavior rules to avoid hidden-instruction execution.

**References / Tools:**

* Online viewers for hidden characters: [SOSCISurvey](https://www.soscisurvey.de/tools/view-chars.php), [ASCII Smuggler](https://embracethered.com/blog/ascii-smuggler.html)
