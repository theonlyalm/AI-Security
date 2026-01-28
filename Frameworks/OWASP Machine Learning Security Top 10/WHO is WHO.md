### 🧪 The "Poisoning" Trio

These all involve making the model "bad," but they happen at different stages.

| Attack | Where it happens | The core difference |
| --- | --- | --- |
| **ML02: Data Poisoning** | Before Training | You mess with the **raw data** (the ingredients) before the model is even built. |
| **ML10: Model Poisoning** | In the "Brain" | You directly edit the **math/weights** (the neurons) inside the model file itself. |
| **ML08: Model Skewing** | During Live Use | You trick the **feedback loop** (user reviews/reports) to slowly bias the model over time. |

---

### 🕵️ The "Privacy" Duo

Both are about stealing info from a model, but the "what" is different.

* **ML03: Model Inversion (The Reconstructor):** The attacker tries to recreate what the training data **looked like** (e.g., "Show me a face that was used to train this").
* **ML04: Membership Inference (The Checker):** The attacker just wants to know if a specific person **was included** (e.g., "Is John Doe's medical record in this dataset? Yes or No?").

---

### 🎭 The "Manipulation" Duo

Both trick the system, but they attack different ends of the process.

* **ML01: Input Manipulation (The Disguise):** You change the **Input** (e.g., put invisible stickers on a stop sign so the AI sees it as a 45mph sign).
* **ML09: Output Integrity (The Middle-man):** The AI sees the stop sign correctly, but you intercept the **Output** and change the text on the screen to say "Go" instead of "Stop."

---

### 🏗️ The "Sourcing" Duo

These involve how you get your model or its tools.

* **ML05: Model Theft (The Heist):** The attacker wants to **steal your finished product** so they can sell it or use it for free without paying you.
* **ML06: Supply Chain (The Sabotage):** The attacker **breaks your tools** (like NumPy or Python libraries) so that anything you build using them is automatically compromised.
* **ML07: Transfer Learning (The Trojan Horse):** A specific version of a supply chain attack where you download a "ready-to-use" model that has a **secret back-door** already hidden inside it.

---

### 💡 The "Cheat Sheet" Summary

* **Input (ML01)** = Messing with the **Questions**.
* **Data (ML02)** = Messing with the **Textbooks**.
* **Model (ML05, ML10)** = Messing with the **Brain**.
* **Output (ML09)** = Messing with the **Answers**.
* **Infrastructure (ML06)** = Messing with the **School**.
