# OT Pre-Surgery Equipment Checklist CLI

> A Python CLI tool to validate pre-surgery equipment readiness in the Operation Theater — built from real clinical experience in Perfusion Technology.

---

## Project Overview

In a high-stakes OT environment, a missed equipment check can cost a life. This tool digitizes the pre-surgery checklist protocol, validates readiness across 6 critical departments, and automatically saves every session to a structured CSV log — making it auditable, reproducible, and data-ready.

Built by a **BSc Perfusion Technology professional** transitioning into Data Analytics — combining domain expertise with Python to solve a real clinical problem.

---

## Problem It Solves

| Without This Tool | With This Tool |
|---|---|
| Paper-based checklists, prone to human error | Interactive terminal-guided validation |
| No audit trail after surgery | Auto-saved CSV log per session |
| No compliance score | Instant % readiness score with clearance verdict |
| Siloed team knowledge | Standardized across operators |

---

## Tech Stack

| Layer | Tool |
|---|---|
| Language | Python 3.x |
| Data Storage | CSV (stdlib) |
| I/O | Terminal CLI (ANSI colors) |
| Libraries | `csv`, `os`, `datetime` — **zero external dependencies** |

---

## How to Run

```bash
# No installations needed — pure Python stdlib
python ot_checklist.py
```

**Menu options:**
```
1. Start New Checklist
2. View Session Logs
3. Exit
```

---

##  Checklist Sections (40 Items)

| # | Section | Items |
|---|---|---|
| 1 | Airway & Ventilation | 8 |
| 2 | Anesthesia Equipment | 7 |
| 3 | Cardiac & Perfusion (CPB) | 10 |
| 4 | Monitoring Equipment | 8 |
| 5 | Surgical Instruments & Sterility | 6 |
| 6 | Safety & Compliance | 7 |

---

##  CSV Log Output

Every session is appended to `ot_checklist_log.csv` with the following schema:

| Column | Description |
|---|---|
| `Timestamp` | Date & time of session |
| `Operator` | Perfusionist / staff name |
| `Patient_ID` | Patient MRN or ID |
| `Surgery_Type` | Type of surgery (e.g., CABG) |
| `Category` | Checklist section |
| `Item` | Equipment item checked |
| `Status` | `PASS` / `FAIL` / `SKIPPED` |

**Sample output:**
```
Timestamp,Operator,Patient_ID,Surgery_Type,Category,Item,Status
2024-07-15 08:32:11,Kusum,PT-2024-0482,CABG,Cardiac & Perfusion (CPB),Heart-Lung Machine primed,PASS
2024-07-15 08:32:14,Kusum,PT-2024-0482,CABG,Cardiac & Perfusion (CPB),Cardioplegia solution prepared,FAIL
```

---

## Clearance Logic

| Compliance Score | Verdict |
|---|---|
| 100% | OT IS CLEARED FOR SURGERY |
| 90–99% | PROCEED WITH CAUTION |
| Below 90% | DO NOT PROCEED |

---

## Key Features

- **Color-coded terminal output** — green/red/yellow for instant visual parsing
- **Session metadata capture** — operator name, patient ID, surgery type
- **Failed item report** — lists every flagged item at the end of the session
- **Append-only log** — never overwrites, always builds a complete audit trail
- **In-terminal log viewer** — view last 20 entries without leaving the app

---

## Roadmap / Future Scope

- [ ] Export session summary as PDF report
- [ ] Power BI dashboard on top of the CSV log
- [ ] Categorize by surgery type for department-wise compliance tracking
- [ ] Role-based checklists (Surgeon / Anesthesiologist / Perfusionist)
- [ ] Integration with hospital HIS/EMR systems

---

## About the Author

**Kusum** — Perfusion Technology (BSc) | Transitioning to Data Analytics

I bring 3+ years of OT clinical experience and am currently building a data analytics portfolio at the intersection of healthcare and data. This project demonstrates my ability to identify real operational problems and solve them with code.

 [LinkedIn](www.linkedin.com/in/kusum-dixit-2a1398246) |  [Portfolio](https://github.com/KusumCPB)

---

## License

MIT License — free to use, adapt, and build upon with attribution.

---

> *"In the OT, there are no second chances. This tool ensures there are no missed checks."*
