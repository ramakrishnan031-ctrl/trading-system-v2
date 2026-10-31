# Not included / pending material

This package contains only files actually available in the current ChatGPT workspace and selected as suitable audit-folder material.

## Architecture outputs visible in the user's screenshot but not supplied as files here
The screenshot lists these Topic-02 artifacts, but their file contents were not uploaded into this ChatGPT workspace and therefore are not included:
- `graph_summary_2026-10-31.json`
- `PACKAGE_DEPENDENCY_MATRIX_2026-10-31.csv`
- `MODULE_INDEX_2026-10-31.csv`
- `IMPORT_EDGES_2026-10-31.csv`
- `static_import_analyzer_2026-10-31.py`
- `PACKAGE_DEPENDENCY...csv` (screenshot shows a separate package dependency CSV; exact filename needs confirmation)

## Deep-audit outputs not present as downloadable files
The activity log refers to DA2, DA3, and DA4 JSON/Markdown reports stored in Claude scratchpad paths, but those files themselves were not uploaded here. Only the summaries in the activity log are available. DA1 completion output is also absent.
- DA2 data-integrity report files: not supplied
- DA3 security/operations report files: not supplied
- DA4 config/tests/history report files: not supplied
- DA1 signals/screening report: not supplied
- Consolidated final findings register: not supplied
- Final master audit report: not supplied
- Final Topic-02 architecture completion report: not supplied

## Not included intentionally
- Raw Claude session/output logs: retained in the ChatGPT workspace but excluded from this GitHub-ready package because they are process transcripts, not finalized audit deliverables, and should receive a separate sensitivity review before publication.
- `ADDENDUM_capital_drift_05-Aug-2026.md`: reference example only; excluded because it may contain sensitive operational details and is not a new audit deliverable.
- Older setup/publishing instructions and intermediate prompts: excluded to avoid mixing obsolete workflow instructions with the audit record. The current master audit prompt is retained under `00_AUDIT_CONTROL/WORKFLOW_REFERENCE/`.

## Publication warning
This is an **interim evidence package**, not a complete audit publication. Before pushing to a public repository, perform a secret and sensitive-data scan of every included file and review the final Git diff. Do not represent this package as the completed full-system audit.
