### 📑 OpenAI "Unlimited" Free Credit Bypass

---

* **Severity:** 🔴 High
* **Vulnerability Type:** Normalization Discrepancy / Validation Bypass
* **Target:** OpenAI Account Registration & Trial Credit System

---

### 📖 Executive Summary

In early 2023, OpenAI offered a **$7–$18 trial credit** for new accounts to encourage API adoption. To prevent abuse, OpenAI required both a unique email and a unique phone number. However, security researcher David Sopas discovered a logic flaw that allowed a single phone number to be reused an infinite number of times. By manipulating the API request with prepended zeros or hidden Unicode characters, attackers could register thousands of accounts and aggregate "unlimited" free credits.

---

### 🔓 The Vulnerability: Bypassing the "Unique" Check

The registration process had a critical weakness in how it handled phone number strings. Normally, if you entered `+351 912 345 678` twice, the system would block the second attempt, stating the number was already in use.

The researcher found that by intercepting the request with a tool like **Burp Suite**, they could modify the `phone_number` parameter to look unique to the database while remaining valid for the SMS delivery system.

#### **Bypass Method 1: Prepending Zeros**

Adding leading zeros made the string unique to the account management system but did not affect the actual dialing of the number.

* `+351912345678` (Account 1)
* `00351912345678` (Account 2)
* `000351912345678` (Account 3)

#### **Bypass Method 2: Fuzzing with REcollapse**

To automate the bypass and find "infinite" variations, the researcher used a tool called **REcollapse**. This allowed for injecting non-ASCII bytes and Unicode characters that the OpenAI API would "cleanse" during the final verification step but treat as "new" during the initial check.

* **Example:** Injecting `\u000a` (newline) or other control characters between digits.

---

### 🔍 Technical Root Cause: Normalization Discrepancy

The flaw was a classic **Normalization Discrepancy**. This occurs when two different parts of an application "see" the same data differently.

1. **The Identity Component (Upstream):** This part checks if the phone number exists in the database. It sees `00123` and `123` as **two different strings** and allows the registration.
2. **The Verification Component (Downstream):** This part handles sending the SMS code. It "sanitizes" the input, removing the leading zeros and extra bytes. It sees both `00123` and `123` as **the same number**, successfully sending the text to the attacker's phone.

---

### 📥 Impact & Remediation

| Feature | Detail |
| --- | --- |
| **Exploit Potential** | Attackers could automate the creation of thousands of accounts to "farm" API credits for free high-volume usage. |
| **Fix** | OpenAI implemented "Early Normalization." Now, the number is sanitized *before* it is checked against the database, ensuring all variations collapse into one identity. |
| **Current Policy (2026)** | OpenAI now strictly limits a phone number to **3 accounts** total, and deleted accounts no longer "free up" slots. |
