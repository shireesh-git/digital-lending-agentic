# CAM Document Model — test corpus

Core information sufficient for an initial credit assessment, plus conditional and additional
evidence. **Nothing is mandatory beyond the core pack, and nothing in the corpus was deleted.**

The machine-readable inventory is `document_inventory.json` (every file: borrower, original path,
current path, category, CAM role, storage category to upload into, likely consumers, SHA-256,
`preserved: true`). Re-check it at any time:

```
python scripts/rationalise_test_documents.py --verify
```

> The pipeline reads `storage/documents/{entity}/{category}/`, not this folder. To test a
> document, upload it into the storage category given by `upload_as_storage_category` in the
> inventory.

## Layout (per borrower)

```
<ENTITY>_<Name>/
  annual-reports/            real annual reports + latest unaudited results (source documents)   CORE
  core-cam/
    financials/              audited / provisional / unaudited financials, quarterly results      CORE
    credit-request/          request note (facility, amount, purpose)                             CORE
    banking-debt/            bank statements, existing facilities, sanction letter, debt schedule CORE
    credit-history/          commercial bureau, CRILC, rating report                              CORE
    borrower-corporate/      KYC (PAN, incorporation), MCA, directors, governance / shareholding    CORE
  conditional/
    security/                valuation report (only where the facility is secured)                CONDITIONAL
    working-capital/         debtor / creditor ageing (only for working-capital facilities)       CONDITIONAL
  additional-evidence/
    projections/  valuation/  legal/  compliance/  site-visit/  insurance/  other/                ADDITIONAL
  platform-document-pack/metadata.json   manifest of the original platform pack (kept in place)
```

Folders are created only where documents exist. No test borrower currently has debtor/creditor
ageing, so `conditional/working-capital/` exists only as a model slot.

## Minimum CAM pack (CORE)

| Area | Documents |
|---|---|
| A. Financial performance | 3 years audited financials (annual reports); latest provisional / unaudited results |
| B. Credit request | Request note: facility, amount, purpose, proposed structure where available |
| C. Banking & existing debt | Existing facilities / debt schedule; recent bank statements |
| D. Credit history | Commercial bureau / CRILC; external rating report |
| E. Borrower / corporate | KYC; MCA master / directors; shareholding (governance report) |

## Conditional evidence

| Area | When required |
|---|---|
| F. Working capital — debtor / creditor ageing | Working-capital facilities, or where receivable / inventory risks are identified |
| G. Security — collateral / valuation | Secured facilities |

## Additional evidence (kept for testing; not required for the initial CAM)

CMA projections, valuation and property documents, CERSAI search, board resolutions, undertakings,
legal history and suits, GST returns, ITR, EPFO, site-visit reports, insurance policies, internal
credit notes, market and web-profile data.

## Mapping to the credit workflow (current capability only)

| Evidence | Flows into |
|---|---|
| Audited / provisional financials | Financial analysis → repayment assessment → resilience assessment → CAM |
| Credit request | Credit assessment (facility details) → credit challenge → credit decision → CAM |
| Banking / existing debt | Repayment assessment, credit history → credit decision |
| Bureau / CRILC / rating | Credit history (CAM section 5) → risk identification |
| KYC / MCA | Validation (new-to-bank document checklist) → borrower profile |
| Security / valuation | Security cover (secondary repayment source) → facility structure → recommendation |
| Debtor / creditor ageing | Risk identification → due diligence → credit challenge → repayment assessment |
