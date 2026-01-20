# Assignment 1: Setup & Debugging Journal
**MLOps & ML Programming (2026)**

## Student Information
* **Name:** Jorn Anders Huijbens
* **Student ID:** 15374963
* **TA Name:** Madelon Bernardy
* **GitHub Repository:** https://github.com/JAHuijbens/UvA-mlops
* **Base Skeleton Used:** [https://github.com/SURF-ML/MLOps_2026/tree/main](https://github.com/SURF-ML/MLOps_2026/tree/main)

---

## Question 1: First Contact with Snellius
1. **Connection Details:**
   - **Command:** `ssh snellius`
   - **Login Node:** int5*
   - **![alt text](assets/image.png)** 

2. **Issues Encountered:**
   - **Error Message:** `n/a`
   - **Resolution:** n/a

3. **Smooth Connection (If applicable):**
   - **SSH Client:** OpenSSH_9.6p1 Ubuntu-3ubuntu13.14, OpenSSL 3.0.13 30 Jan 2024
   - **Prior Experience:** using ssh for INF course webtech and work on das5 during DPP
   - **Preemptive Steps:** created an ssh alias for ease of use

---

## Question 2: Environment Setup
1. **Setup Sequence:**
   - **Commands:** 
   ```
   7  module load 2025
   8  module avail python
   9  module load Python/3.13.1-GCCcore-14.2.0
   10  module avail matplotlib
   11  module load matplotlib/3.10.3-gfbf-2025a
   12  python3 -m venv .venv
   13  source .venv/bin/activate
   14  pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
   15  python3
   16  pip3 install packaging
   17  history
   ```
   - **Full Venv Path:** `/home/scur2308/UvA-mlops/.venv`

2. **Pip Install Torch:**
   - **Duration:** 3 min
   - **Warnings:** n/a
   - **Venv Size:** `4.5G`

3. **Mistakes/Unexpected Behavior:**
n/a

4. **Verification:**
   - **Output:** ```
   PyTorch: 2.9.1+cu130
   CUDA available: False
   ```
   - **Explanation:** its expected that CUDA is not available, login node does not have access to a gpu

---

## Question 3: Version Control Setup
1. **GitHub URL:** [\[Link\]](https://github.com/JAHuijbens/UvA-mlops)
2. **Authentication:** SSH + my ssh agent did not want to forward my keys to server, so manually uploaded set of keys to server to auth with github
3. **.gitignore:**
   - **Contents:** 
   ```
   __pycache__/
   *.pyc
   experiments/logs/
   experiments/results/
   .env
   .DS_Store
   *.egg-info/
   ```
   - **Important items to include:** credentials, temporary files, build results, logs etc
   - **README info:** should not include snellius access info, should include how to load dependencies
4. **Git Log:** `839bb48 (HEAD -> main, origin/main) ass1 commit`

---

## Question 4: Your First Batch Job (Slurm)
1. **Files Provided:** slurm.sh, mlops_assignment1.out
2. **Job ID & Stats:** ```
   (.venv) scur2308@int6:~/UvA-mlops/assignment1$ sacct -j 18489843 --format=JobID,Start,End,Elapsed,State
   JobID                      Start                 End    Elapsed      State 
   ------------ ------------------- ------------------- ---------- ---------- 
   18489843     2026-01-20T11:30:05 2026-01-20T11:30:13   00:00:08  COMPLETED 
   18489843.ba+ 2026-01-20T11:30:05 2026-01-20T11:30:13   00:00:08  COMPLETED 
   18489843.ex+ 2026-01-20T11:30:05 2026-01-20T11:30:13   00:00:08  COMPLETED
   ```
3. **Submission Problem:** n/a
4. **Verification:** see output in mlops_assignment1.out
5. **Login vs Batch:** Running on login node uses shared resources, batch job resources are specifically allocated to you
6. **Why Clusters?:** To run many jobs at one time, to have access to increased compute
---

## Question 5: Reflection & Conceptual Understanding
1. **The Filesystem:**
   - **I/O Performance:** Snellius makes use of distributed file system for storage so across many small 100k files it would need to do many metadata lookups whereas 1 big file requires only 1 metadata lookup.
   - **Mitigation Strategies:** combine files into one larger file, or use local filesystem of node
   - **Dataset Versioning:** We could version control datasets by storing patches for versions, and applying them in memory
2. **Reproducibility:**
   1. randomness may introduce different results between runs, resolved by setting set seed
   2. speed at which files are read may differ between runs, fixed by using generator to determine which worker provides next file
   3. may be differences in library version, hardcode the library versions
3. **Conda vs venv vs uv:** conda makes many small files, bad for i/o performance, uv is quick and has a pip-like interface, uv does however save its own python executable, venv is an official tool of the python foundation and uses familiar commands, venv is also easily automated with scripts

---

## Question 6: Package Integrity
1. **ModuleNotFoundError:** forgot to install workspace dependencies, resolved by installing workspace dependencies
2. **Import Abstraction:** importing from ml_core.data ensures public api access and avoids importing private APIs
3. **Pytest Result:** 
   ```
   =================================================================== test session starts ====================================================================
   platform linux -- Python 3.13.1, pytest-8.3.5, pluggy-1.5.0
   rootdir: /gpfs/home6/scur2308/UvA-mlops
   configfile: pyproject.toml
   plugins: xdist-3.6.1
   collected 1 item                                                                                                                                           

   tests/test_imports.py .                                                                                                                              [100%]

   ==================================================================== 1 passed in 13.30s ====================================================================
   ```

---

## Question 7: The Data Pipeline
1. **Implementation:** `[Paste __getitem__ method]`
2. **Local Pytest:** `[Paste output of pytest tests/test_pcam_pipeline.py]`
3. **CI Pipeline:**
   - **Screenshot:** ![GitHub Actions Tab](assets/github_actions.png)
   - **Reflection:** [CI vs Local discrepancies]
4. **Sampling Math:** [Average positives with vs without WeightedRandomSampler]
5. **EDA Plots:**
   - ![PCAM Intensity Outliers](assets/pcam.png)
   - [Additional plots as requested]

---

## Question 8: Model Implementation (MLP)
1. **Forward Pass:** [Error details + dimension calculation for (3, 96, 96)]
2. **Weight Updates:** [Why check backprop explicitly?]
3. **Test Output:** `[Paste output of pytest on the relevant file]`

---

## Question 9: Training Loop & Loss Visualization
1. **Training Execution:** [Method used + Node ID (gcnXX)]
2. **Loss Visualization:**
   - **Plot:** ![Training and Validation Loss](assets/loss_plot.png)
   - **Trajectory Analysis:** [Healthy curve? Trajectory hypothesis?]
3. **Most Frustrating Error:**
   - **Error Message:** `[Traceback]`
   - **Debugging Steps:** [How you resolved it]

---

## Final Submission Checklist
- [x] Folder contains .md file and assets/ folder?
- [x] Name and Student ID on page 1?
- [x] All code/terminal snippets are in backtick blocks?
- [x] All images use relative paths (e.g., assets/pcam.png)?
- [x] Slurm .sh and .out files included in the .zip?