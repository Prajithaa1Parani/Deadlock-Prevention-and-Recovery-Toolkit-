# 🧠 Deadlock Prevention and Recovery Toolkit

**Project:** Deadlock Prevention and Recovery Toolkit — Banker's Algorithm & Resource Allocation Graph  
**Author:** Prajithaa Parani  
**Course:** B.Tech Gen.AI, Lovely Professional University

---

## 🚀 Project Summary (copy-paste ready)
A lightweight web-based simulator that **detects**, **prevents**, and **recovers** from deadlocks in real-time.  
Implements the **Banker’s Algorithm** for safety checks, shows a live **Resource Allocation Graph (RAG)** using SVG, and provides an interactive interface to:
- create processes/resources,
- edit `Available`, `Max`, `Allocation` matrices,
- compute `Need` and check safety,
- simulate requests,
- abort a process to recover from deadlock.

The UI is pure HTML/CSS/JS and is copy-paste runnable as a single `index.html`.

---

## 📁 Repository Structure

```bash
deadlock-prevention-and-recovery-toolkit/
│
├── index.html                # Main project file (HTML + CSS + JS combined)
├── README.md                 # Complete project documentation
├── LICENSE                   # Optional (MIT or your preferred license)
│
├── assets/                   # Folder for visuals (optional)
│   ├── demo.gif
│   ├── screenshot1.png
│   └── screenshot2.png
│
├── js/                       # Folder for scripts (optional)
│   └── main.js               # Banker's Algorithm & graph logic
│
├── css/                      # Folder for styles (optional)
│   └── style.css             # UI design & styling
│
└── docs/                     # Folder for reports & presentations (optional)
    ├── system_design_report.pdf
    ├── algorithm_explanation.pdf
    └── project_presentation.pptx
```


## 🧩 Files to include
- `index.html` — the full app (HTML, CSS, JS combined).  
  > Use the HTML you already have (the UI with matrices, Banker's logic, graph drawing & example).  
- `README.md` — (this file).  
- `LICENSE` — optional.

---

## ✅ Features (complete)
- Create any number of processes (P) and resource types (R).  
- Dynamic input fields for:
  - `Available` vector (R length)
  - `Max` matrix (P x R)
  - `Allocation` matrix (P x R)
- Auto-compute `Need = Max - Allocation`.  
- **Banker’s safety algorithm** to check safe / unsafe states and produce safe sequence when available.  
- Simulate `request` from any process; the algorithm tentatively allocates, checks safety, grants or denies accordingly (prevention).  
- Visual **SVG Resource Allocation Graph**:
  - Processes as circles at top
  - Resources as rounded rectangles at bottom
  - Blue arrows = allocation (resource → process)
  - Orange arrows = request (process → resource)
  - Labels show allocation counts
- `Example` prefill (classic Banker's demo) to test known safe/unsafe cases.  
- `Recovery` button: abort the selected process to free its resources (simple recovery strategy).  
- Input validation: prevents Allocation > Max (auto-adjusts), prevents requests > Need, prevents allocation if Available insufficient.  
- Friendly messages and status labels (safe/unsafe, granted/denied, adjustments made).

---

## ⚙️ How to run (very simple)
1. Put `index.html` (the single HTML file) in a folder.  
2. Open `index.html` in any modern browser (Chrome/Edge/Firefox).  
3. Interact:
   - Set process/resource counts → **Create Matrices**
   - Fill tables or click **Example**
   - Click **Compute Need & Check Safety** to view `Need` and safety result
   - Use **Simulate a resource request** to try allocation
   - Use **Draw Allocation Graph** to visualize
   - Use **Recovery** to abort a process

No server required.

---

## 📚 Complete explanation (detailed, but simple)

### Data structures used (JS variables)
- `P` — number of processes
- `R` — number of resource types
- `Available` — array length R: how many instances of each resource free
- `Max` — array of arrays P x R: maximum demand per process
- `Allocation` — array of arrays P x R: currently allocated units per process
- `Need` — array of arrays P x R: computed as `Max - Allocation`

### Banker's Algorithm (safety check) — step-by-step
1. Compute `Need[i][j] = Max[i][j] - Allocation[i][j]` for all processes i and resources j.
2. Initialize `Work = Available` (copy) and `Finish[i] = false` for all processes.
3. Find a process `i` such that `Finish[i] == false` and `Need[i] <= Work` (component-wise).
4. If found:
   - `Work = Work + Allocation[i]` (release resources after simulated completion)
   - `Finish[i] = true`
   - append `i` to safe sequence
   - repeat step 3
5. If all `Finish[i]` become `true`, system is **SAFE**; safe sequence returned. Otherwise **NOT SAFE**.

### Handling a request (per process `pid` and request vector `req`):
1. Verify `req <= Need[pid]`. If not, deny.
2. Verify `req <= Available`. If not, request must wait.
3. Tentatively:
   - `Available = Available - req`
   - `Allocation[pid] = Allocation[pid] + req`
   - `Need[pid] = Need[pid] - req`
4. Run safety check:
   - If safe → permanently grant (state stays)
   - If unsafe → rollback tentative changes, deny to **prevent** unsafe state

### Recovery (simple abort)
- Choose a process `pid` to abort.
- `Available += Allocation[pid]`
- Set `Allocation[pid] = 0`, `Need[pid] = 0`, `Max[pid] = 0` (or mark process terminated)
- Recompute safety

---

## 🔧 Implementation notes & helpful comments to include in `index.html` JS
- Always validate that `Allocation[i][j] <= Max[i][j]`. If user mistakenly sets Allocation greater than Max, auto-correct and warn.
- Use `Number.parseInt(..., 10)` or `+value` to parse ints (and default to 0 on NaN).
- When drawing SVG graph, recompute positions on every change to keep visualization synchronized.
- Keep the `Need` matrix updated after any change to `Max`, `Allocation`, or `Available`.
- Provide clear UI colors and text for states:
  - `.status.safe` → green text
  - `.status.unsafe` → red text
  - Request results use subtle color cues

---

## 🧪 Example (useful to copy into UI via the "Example" button)
- `Available = [3, 3, 2]`  
- `Max = [
  [7,5,3],
  [3,2,2],
  [9,0,2],
  [2,2,2],
  [4,3,3]
]`  
- `Allocation = [
  [0,1,0],
  [2,0,0],
  [3,0,2],
  [2,1,1],
  [0,0,2]
]`
- After compute → `Need` and safe sequence should show: **P1 → P3 → P4 → P0 → P2** (classic demonstration).  
(This is pre-filled in the example function in the provided HTML.)

---

## 🛠️ Troubleshooting & FAQ
**Q: I entered Allocation > Max for some cell — what happens?**  
A: UI auto-adjusts Allocation down to Max for safety and informs you.

**Q: Granting a request says “Not enough available” but I think there are enough?**  
A: Check `Available` vector; simulated tentative allocation reduces `Available`. Also ensure request ≤ Need.

**Q: The graph arrows overlap and look messy for large P/R**  
A: This demo targets small P/R (practical for learning). For larger P/R, consider improved layout algorithms (force-directed graph) or grouping.

**Q: I aborted a process but the UI still shows it**  
A: Aborting sets its `Max` and `Need` to 0. You can also reload the page or implement a “remove process” feature to hide terminated processes.

---

## 📜 License
MIT License
Copyright (c) 2025 Prajithaa Parani
