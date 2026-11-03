# NovaTel Performance Optimization Lab

**Data Factory & Digital Analytics for Enterprise - Performance Optimization & Scalability**
Hands-on activity · 1 h · mixed technical backgrounds

Participants run a real benchmark in Python, interpret the results, and submit an evidence-based recommendation. Every performance number comes from the participant's own runtime. Nothing is hard-coded.

---

## What's in the package

| File | Who uses it | What it does |
|---|---|---|
| `NovaTel_Interactive_Performance_Lab.ipynb` | Participants (Google Colab) | Runs the real experiment. Five missions, interactive checks, charts, recommendation form, and exports `novatel_results_<name>.json`. |
| `NovaTel_Performance_Command_Center.html` | Participants (browser) | Imports the results JSON. Scored missions, decision challenges, executive summary, and JSON / CSV / printable report export. **It does not run Python.** |
| `NovaTel_Trainer_Dashboard.html` | Trainer (browser) | Imports many participant submissions at once. Shows class KPIs, decision split, question difficulty and a participant table, with CSV / JSON export. |
| `telecom_revenue_100k.csv` | Participants | Synthetic dataset: 100,000 transactions, 5 regions, 4 service types. Distribute it alongside the notebook. |

No installation, API keys or paid services are needed. The HTML files are single, self-contained pages. Open them by double-clicking, in Chrome or Edge.

---

## How the pieces connect

```
Colab notebook ──(runs real benchmark)──► novatel_results_<name>.json
        │                                          │
        │                                          ▼
        │                        Command Center (import) ──► scored missions
        │                                          │
        │                                          ▼
        │                            novatel_submission_<name>.json
        │                                          │
        └── (notebook-only file also accepted) ───►▼
                                     Trainer Dashboard (import many)
```

---

## Participant guide (≈ 22 minutes)

<div dir="rtl">

**بالعربية باختصار:** افتح الدفتر في Colab ← شغّل كل الخلايا ← ارفع ملف البيانات ← أجب عن الأسئلة وأكمل نموذج التوصية ← نزّل ملف النتائج JSON ← افتح لوحة القيادة واستورده ← أكمل المهام ← صدّر ملف التسليم للمدرب.

</div>

| Step | Where | Time |
|---|---|---|
| 1. Open <https://colab.research.google.com> → **File → Upload notebook** → choose the `.ipynb`. | Colab | 1 min |
| 2. Type your name in the **Participant** cell, then **Runtime → Run all**. Upload `telecom_revenue_100k.csv` when asked. | Colab | 2 min |
| 3. **Mission 01–03**: read the KPI cards, answer the self-checks, click **Re-run experiment** at least once, and try the size selector. | Colab | 6 min |
| 4. **Mission 04–05**: answer the decision checks, fill in the recommendation form, and click **Generate summary & save results**. | Colab | 4 min |
| 5. Run the **⬇️ Download results** cell. This saves `novatel_results_<name>.json`. | Colab | 1 min |
| 6. Open `NovaTel_Performance_Command_Center.html` and **import** the JSON. Complete Missions 01–05 there. These are the scored attempts. | Browser | 7 min |
| 7. In Mission 05, click **Submission JSON (for trainer)** and send it to the trainer. Optionally download the CSV or print the one-page report to PDF. | Browser | 1 min |

**Tips**
- Retries are unlimited. A first-attempt correct answer earns full points, a second attempt earns 50%, and later attempts earn 25%. After two wrong tries you can reveal the answer for 0 points and keep going.
- To re-measure, re-run the notebook cell, download again and re-import. Your answers are kept.
- Progress in the Command Center is saved in your browser. **Reset lab** clears it.

---

## Trainer guide

**Before the session**
1. Share the notebook, the CSV and `NovaTel_Performance_Command_Center.html`. A shared drive or LMS works.
2. Decide how participants will return `novatel_submission_<name>.json`, for example an LMS assignment, a shared folder, or a Teams/Slack channel.
3. Optional: do a dry run yourself, then import your own submission into the Trainer Dashboard to see the views.

**During the session**
- Open `NovaTel_Trainer_Dashboard.html` and drag in submissions as they arrive. You can re-import at any time. A newer file from the same participant replaces the older one.
- **Question difficulty** shows the share correct on the first try. Low bars are good debrief topics.
- **Executive decision — first choice** shows how many picked A, B or C before feedback. B is the evidence-based answer. A and C make good discussion starters.
- **Measured speedup by participant** varies with hardware and load. Use it to show that measurements are environment-specific, not to rank people.

**After the session**: **Results CSV** gives one row per participant with per-question points. **Merged JSON** keeps the full detail.

**Debrief anchor (from the original lab):** a correct recommendation is conditional. Vectorization is preferable *for this column operation under the tested conditions*, not automatically for every pipeline workload.

---

## Scoring (Command Center, 100 points)

| Component | Points | Rewards |
|---|---:|---|
| Evidence integrity | 5 | Results file passes the recomputation checks: 7 repetitions, medians match the raw timings, speedup / reduction / throughput recompute correctly, `np.allclose` passed |
| Mission 01 · Data understanding | 10 | What quality checks prove; sanity-checking the discounted total |
| Mission 02 · Interpreting the experiment | 20 | Speedup vs time reduction; reporting variable timings honestly |
| Mission 03 · Scalability reasoning | 15 | Why speedup changes with size; how to use a hypothetical estimate |
| Mission 04 · Executive decisions | 30 | Deployment decision (A/B/C); memory risk; cost vs pipeline share (Amdahl's law) |
| Mission 05 · Recommendation quality | 20 | Solution consistent with evidence, specific risk, concrete validation test, recommendation cites a measured number |

Only 5 points come from simply running the code. The rest rewards interpretation, quality checks and decisions.

**Achievement levels:** Analyst (< 60) · Performance Specialist (60–84) · Optimization Advisor (≥ 85).

The original 10-point rubric is kept at the end of the notebook for trainers who prefer manual grading.

---

## Scientific integrity — what the lab preserves

- **Same logic as the original lab.** Method A is a Python loop appending `value * 0.95`. Method B is `(df['revenue'] * 0.95).to_numpy()`. Both run on the same input.
- **Timing protocol.** `time.perf_counter()`, one untimed warm-up per method, then 7 alternating timed repetitions, reporting the **median**.
- **Correctness.** `np.allclose(row, vector, rtol=1e-12, atol=1e-9)`, plus the maximum absolute difference.
- **Metrics.** Speedup = A/B. Time reduction = (1 − B/A) × 100. Throughput = records / seconds. All divisions are guarded against zero.
- **Scope.** Only the computation is timed. CSV loading, charts and the wider pipeline are not.
- **Scalability.** Real benchmarks at 10k, 50k and 100k rows (the first *n* rows). Nothing is extrapolated in the charts.
- **Hypothetical vs measured.** The 12M-row figure is labelled **HYPOTHETICAL** everywhere. Extrapolating from each measured size shows how much the estimate itself varies.
- **Noise warnings.** A warning appears when the run-to-run spread is above 15%, when Method B takes under 1 ms (timer and overhead effects), or when repeated experiment runs disagree by more than 25%.
- **Tamper evidence.** The Command Center recomputes medians and ratios from the raw timings in the file, so a hand-edited value fails the integrity check.

---

## File formats (for reference)

- **`novatel-lab-results/v1`** comes from the notebook. It contains `dataset` (profile and quality checks), `benchmark` (raw timings, medians, ratios, correctness), `benchmark_history`, `scalability[]`, `simulation` (hypothetical), `environment`, `notebook_self_checks` and `notebook_recommendation`.
- **`novatel-lab-submission/v1`** comes from the Command Center. It contains `participant`, `score` (total, level, breakdown), `missions_completed`, `decision`, `recommendation`, `key_metrics`, `integrity`, `executive_summary`, per-question `answers`, and the full `measurements` (the results file above). Importing a submission back into the Command Center restores that participant's progress.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Widgets (buttons, dropdowns) don't appear in the notebook | The lab still works. Each check prints instructions for the text fallback, e.g. `answer('m1_q1', 'B')` and `save_recommendation(my_rec)`. |
| The Download JSON link in the notebook does nothing | Run the **⬇️ Download results** cell, which uses Colab's own downloader. As a last resort, open the JSON from the Colab file panel, copy the text, and use **Paste the JSON text instead** in the Command Center. |
| "Could not import this file" | Use the JSON produced by the notebook. The message lists what's missing, e.g. "run Mission 03". |
| Integrity check shows FAIL | The file was edited, or comes from a modified notebook. Re-export from the notebook without editing. |
| Fonts look plain offline | The pages load Inter from Google Fonts and fall back to system fonts with no loss of function. |
| Progress lost | Progress lives in the browser's local storage. Private windows and cleared site data reset it. Re-import the submission JSON to restore it. |
| Notebook not on Colab | Works in local Jupyter too. Put the CSV next to the notebook and run all cells. |

---

## Optional enhancement: real-time class view (not included)

The delivered dashboard is **standalone and file-based**. It does not synchronise in real time. If you want submissions to appear live, you need a small shared backend. A minimal, low-cost design:

1. **Collector.** A Google Sheet with an Apps Script web app (`doPost`) that appends each submission as a row and stores the JSON in a column. Alternatives are Supabase or Firebase Firestore with a single `submissions` table and an insert-only rule.
2. **Sender.** Add a **Submit to class** button in the Command Center that `POST`s the submission JSON to the collector URL. Keep the file download as a fallback.
3. **Reader.** Have the Trainer Dashboard poll the collector's read endpoint (for example `doGet` returning JSON) every 15–30 seconds and feed the results into the existing `importFiles` pipeline.
4. **Setup and governance.** Restrict the collector to your organisation's accounts, add a session code to separate cohorts, store only name, team and lab results, and delete the data after the course.

This needs an account on the chosen platform and about an hour of setup. It is intentionally not built in, so the core lab runs anywhere with no accounts.

---

*NovaTel is a fictional training company; the dataset is synthetic.*
