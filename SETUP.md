# Setup Guide – Electricity Theft Detection

Follow these steps once to run the project on your own laptop.

---

## 1. Install the tools (skip what you already have)

| Tool | Mac | Windows |
|---|---|---|
| **Python 3.12** | `brew install python@3.12 libomp` (needs Homebrew: https://brew.sh) | Download Python **3.12** from https://www.python.org/downloads/ and tick **"Add Python to PATH"** while installing |
| **Git** | Comes with Xcode Command Line Tools (`git --version`) | https://git-scm.com/download/win |
| **VS Code** | https://code.visualstudio.com | same |
| **VS Code extensions** | Python, Jupyter | same |

> Use Python **3.12**, not the newest version – some ML libraries (SHAP) may not support the newest one yet.

---

## 2. Get the code

```bash
git clone <REPO_LINK>
cd electricity-theft-detection
```

Then open this folder in VS Code (**File → Open Folder**). Open the **whole folder**, not a single file.

---

## 3. Create the virtual environment and install libraries

**Mac:**
```bash
python3.12 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Windows:**
```bash
py -3.12 -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

You should see `(venv)` at the start of the terminal line.
Every time you open the project again, activate it first (`source venv/bin/activate` or `venv\Scripts\activate`).

> If `pip install -r requirements.txt` fails on Windows, run this instead:
> `pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn xgboost shap streamlit jupyter ipykernel joblib`

---

## 4. Download the dataset (not on GitHub – it is too big)

1. Log in to https://www.kaggle.com
2. Open the **"SGCC Electricity Theft Detection"** dataset by **bensalem14**:
   https://www.kaggle.com/datasets/bensalem14/sgcc-dataset
3. Download and unzip it.
4. Put the file **`data set.csv`** into the project's **`data/`** folder.
5. Rename it to **`data.csv`**.

---

## 5. Recreate the cleaned data

In VS Code, open each notebook below, click **Select Kernel** (top right) → choose the **venv** one, then click **Run All**:

1. `notebooks/02_clean_data.ipynb` → creates `data/clean.csv`
2. `notebooks/04_features.ipynb` → creates `data/features.csv`

The other notebooks (03 EDA, 05 models, 06 unsupervised, 07 SHAP) can then be run in any order.

> **Kernel error ("requires the ipykernel package")?** You picked the wrong Python. Choose the kernel whose path contains `venv`.

---

## 6. Run the app

From the project folder, with `(venv)` active:
```bash
streamlit run app/streamlit_app.py
```
It opens at http://localhost:8501. Stop it with **Ctrl + C**.

The app works even before step 4–5, because the trained model (`models/`) and demo data (`app/sample_data.csv`) are already in the repo.

---

## Project structure

```
electricity-theft-detection/
├── data/          dataset files (not on GitHub, created in steps 4–5)
├── notebooks/     01 explore → 02 clean → 03 EDA → 04 features → 05 models → 06 unsupervised → 07 SHAP
├── src/           features.py, preprocess.py (shared by notebooks and app)
├── models/        theft_model.joblib, clusters.joblib
├── app/           streamlit_app.py, sample_data.csv
├── reports/       figures/ and result tables
└── requirements.txt
```

---

## Working together with Git

Before you start working each day:
```bash
git pull
```

After you finish a change:
```bash
git add .
git commit -m "short description of what you changed"
git push
```

- Don't edit the same notebook as someone else at the same time – notebooks are hard to merge.
- Never commit `venv/` or `data/` (they are already in `.gitignore`).
