### 🛡️ AI Bypass: The "Intended Feature" Loophole

---

* **Vulnerability Type:** Logic Flaw / Authentication Bypass
* **Target:** Unnamed Photo Sharing Application (Android/iOS)

---

### 📖 Executive Summary

In early 2024, researcher Ishwar Kumar disclosed a logical vulnerability in a photo-sharing application's facial recognition system. The app used AI to map user faces for auto-tagging in group photos. While the app enforced a "live" facial scan during registration to prevent spoofing, Kumar discovered a fallback mechanism—an "Intended Feature"—that completely negated the security check. By simply tapping outside the facial detection box, a user could force the app to accept a static image from the gallery, allowing attackers to use deepfakes or stolen photos to impersonate others.

---

### 🛠️ Technical Walkthrough

#### **1. The Security Barrier (The Front Door)**

The application required a mandatory facial recognition step after phone/email verification. The AI was designed to verify:

* The subject is human.
* The face is looking straight at the camera.
* **Liveness:** It rejected static physical photos held up to the camera (likely due to blur or depth anomalies).

#### **2. The Bypass ( The Back Door)**

Kumar found that the application developers had included a fallback option for usability, which became the vulnerability.

* **Action:** During the facial scan, the user intentionally **taps outside the facial feature box**.
* **Reaction:** The app interrupts the live scan and offers an option to **"Import from Gallery."**
* **The Flaw:** Once an image is imported from the gallery, the strict "liveness" checks (verifying the user is physically present) are skipped. The AI only checks if the image contains a human face looking straight ahead.

#### **3. The Exploit**

An attacker could simply:

1. Find a clear photo of a target (or generate a deepfake).
2. Reach the facial verification step.
3. Tap out of the box  Import the target's photo.
4. **Access Granted:** The attacker is now verified as the target.

---

### 🚨 Impact Assessment

| Risk Category | Consequence |
| --- | --- |
| **Privacy Invasion** | The attacker gains access to all private group photos where the victim's face appears, as the AI auto-tags the "verified" face in them. |
| **Identity Theft** | Attackers can create verified profiles using stolen identities. |
| **Admin Takeover** | If the same mechanism protects admin accounts, an attacker could bypass high-level authentication using a public photo of an administrator. |
| **Profile Poisoning** | Creating multiple accounts with the same face could confuse the AI model, polluting the tagging data for legitimate users. |

---

### 🛡️ 2026 Perspective: Liveness Detection Evolution

By 2026, relying solely on client-side logic (like "tap to upload") for identity verification is considered a critical anti-pattern. Modern standards, such as **FIDO2** and **ISO 30107-3 (Pad)**, require:

* **Active Liveness:** Prompting the user to move or blink in a random sequence.
* **Passive Liveness:** analyzing micro-movements and skin texture reflection (subsurface scattering) that static images lack.
* **Server-Side Validation:** Never trusting the client to "switch" authentication methods. If a live scan fails, the fallback should be a manual review, not an unverified upload.
