# Financial Statements Builder — an agent skill

Turns a trial balance, a full general ledger, or live ERP data into a complete, audit-ready
financial statements bundle — the way a Big 4 engagement partner would prepare it.

**Author:** Parin Shah, CA, CPA — CFO | [linkedin.com/in/parinshah-cfo](https://linkedin.com/in/parinshah-cfo)

## What it produces
- Balance Sheet, Statement of Profit and Loss (with OCI), Statement of Changes in Equity, Cash Flow
- Notes to accounts numbered in face-statement order, every note total formula-linked to its face line
- Material accounting policies, critical estimates and judgements
- Related party note **plus** an RPT compliance matrix (Companies Act s.188 / Rule 15, s.177,
  SEBI LODR Reg 23 — approvals, materiality, arm's length, ordinary course)
- Consolidation with numbered elimination entries, goodwill, NCI and FCTR
- Multi-currency translation (Ind AS 21 / IAS 21 / ASC 830) and convenience translation
- Prior-period comparison with variance flags, Schedule III's 11 ratios, management metrics and a
  one-page board summary
- A hard-gate Checks tab (32 checks) — BS balances, P&L → equity, CF → cash, note → face, eliminations
- Signature blocks (auditor, FRN, partner, membership no., UDIN, directors with DIN, CFO, CS, place, date)
- An Open Points list — anything uncertain is called out, never plugged

## Frameworks
Ind AS (Schedule III Division II and III), AS / Indian GAAP (Schedule III Division I), IFRS
(IAS 1, with IFRS 18 from 2027), US GAAP (incl. Reg S-X for registrants). Sector overlays for
NBFCs, real estate, flex office / lease-heavy businesses, manufacturing, IT/SaaS, GCCs, GIFT City
IFSC units, pharma, retail/D2C, infrastructure, startups and Section 8 entities.

## Data sources
Excel/CSV trial balance, GL/day book, or API: Zoho Books, Tally, QuickBooks Online, Xero, SAP
S/4HANA, NetSuite, Dynamics 365 BC. Read-only on the source system.

## Structure
```
financial-statements-builder/
├── SKILL.md                         # the skill prompt (workflow, principles)
├── references/                      # loaded on demand, stage by stage
│   ├── 01-data-ingestion.md
│   ├── 02-ledger-classification.md
│   ├── 03-framework-formats.md
│   ├── 04-notes-and-policies.md
│   ├── 05-related-parties.md
│   ├── 06-consolidation-and-fx.md
│   ├── 07-validation-checks.md
│   ├── 08-ratios-and-analysis.md
│   ├── 09-output-format.md
│   └── 10-sector-overlays.md
├── scripts/
│   ├── classify_ledgers.py          # first-pass FSLI mapping with confidence, sign and duplicate flags
│   └── validate_fs.py               # hard-gate validator for the finished workbook
└── templates/
    ├── sample_trial_balance.csv
    ├── rpt_compliance_matrix.csv
    └── open_points.csv
```

## Install
- **Claude (app / Claude Code):** copy the `financial-statements-builder` folder into your skills
  directory (e.g. `~/.claude/skills/`) or upload the zip as a skill.
- **Other agents:** point the agent at `SKILL.md`; it references the other files by relative path.

## Use
> "Here is our FY26 trial balance (attached). Prepare Ind AS standalone financial statements with
> notes, RPT disclosure and ratios. Listed on NSE. Rounding in ₹ lakhs."

> "Pull the TB from Zoho Books for FY26 for both entities, consolidate with the Dubai subsidiary
> (functional currency AED) and give me the consolidated set in INR with a USD convenience translation."

Try the scripts on the sample:
```
python scripts/classify_ledgers.py templates/sample_trial_balance.csv mapping.csv
python scripts/validate_fs.py your_financials.xlsx
```
The sample TB deliberately contains a suspense balance, a debit-balance creditor, a credit-balance
debtor and a duplicate bank ledger so you can see the flags fire.

Requirements: Python 3.9+, pandas, openpyxl.

## Important
This skill drafts financial statements and compliance aids. It does not replace the judgement of
the preparer, the board or the statutory auditor, and it is not legal advice. Regulatory
thresholds (RPT materiality, Rule 15 limits, rounding, MSME) change — the skill is instructed to
verify them against the current notifications before use.

## Licence
MIT
