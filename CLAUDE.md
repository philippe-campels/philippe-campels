# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

Workspace for SAP ABAP and SAP consulting work. ABAP sources are stored as plain `.abap` text files (one object per file, named after the SAP object, e.g. `Z_REPORT.abap` = report `Z_REPORT`). There is no abapGit layout, no build system and no SAP system connected to this container: code is written and reviewed here, then copied into SE38 / SE80 / ADT on the target system and moved through transports.


## Commands

Nothing can be compiled, activated or unit tested here (no SAP backend). Validation options available in this environment:

- **Lint / syntax check**: MCP tool `mcp__SAP_ABAP_Docs_Marian_Zeis__abap_lint` with the file content as `code`. Pass `version: "Standard"` for on-premise ECC / S/4HANA code (the default `"Cloud"` flags every classic statement as non-cloud). Limit 50 KB per call. abaplint has no DDIC, so it does not know SAP tables or data elements; treat `unused_variables` on `TABLES` / selection-screen declarations as noise.
- **Release availability of a syntax feature**: `mcp__SAP_ABAP_Docs_Marian_Zeis__abap_feature_matrix`.
- **Is an SAP object released / deprecated / its successor (Clean Core level)**: `mcp__SAP_ABAP_Docs_Marian_Zeis__sap_get_object_details` (set `system_type` to `on_premise` or `private_cloud` as appropriate).
- **Keyword docs, Clean ABAP, DSAG guide, SAP Help, SAP Community**: `mcp__SAP_ABAP_Docs_Marian_Zeis__search` then `fetch`. Use `abapFlavor: "standard"` for on-premise language questions.
- ABAP unit tests (`FOR TESTING` classes) can be written here but only run on the SAP system (SE80 / ADT, Ctrl+Shift+F10).

## ABAP conventions used in this repo

- Modern syntax is used (7.40 SP08+ / 7.50+): inline declarations `DATA(...)`, `NEW`, `VALUE`, `CONV`, new Open SQL with `@` host variables and comma separated field lists. Do not downgrade to older syntax unless the target release requires it; ask for the release if unknown.
- Report structure: types, selection screen, local class definition / implementation, then events (`INITIALIZATION`, `START-OF-SELECTION`) that only instantiate the class and call it.
- Data correction reports must keep a test mode that defaults to ON (`P_TEST DEFAULT 'X'`) and print a summary of candidates / updated / errors.
- Customer namespace: `Z*` / `Y*` objects, local classes `lcl_*`, prefixes `lv_` / `ls_` / `lt_` / `gr_`, `s_` select-options, `p_` parameters.

## SAP tooling available to Claude in this environment

Use these instead of answering SAP questions from memory:

- **SAP ABAP Docs / SAP Docs MCP servers** (`mcp__SAP_ABAP_Docs_Marian_Zeis__*`, `mcp__SAP_Docs_Marian_Zeis__*`): search, fetch, abap_lint, feature matrix, object release state, SAP Community and Discovery Center search, `ui5_version_diff`.
- **SAP Fiori MCP server** (`mcp__plugin_sap-fiori-mcp-server_fiori-mcp__*`): generate Fiori Elements apps (OData or CAP), modify existing apps (`list_functionality` then `get_functionality_details` then `execute_functionality`), download OData metadata, search Fiori / UI5 docs. Related skills: `sap-fiori-mcp-server:*`.
- **Skills for ABAP delivery and documentation** (invoke via the Skill tool): `anthropic-skills:livraison-code-abap`, `anthropic-skills:documentation-objets-abap`, `anthropic-skills:revue-ordre-transport`, `anthropic-skills:traduction-textes-abap`, `anthropic-skills:documentation-itf-sapscript`, `anthropic-skills:analyse-incident-sap`, `anthropic-skills:capitalisation-incident-sap`, `anthropic-skills:capitalisation-connaissance-sap`, `anthropic-skills:sap-api-policy-evidence`.
- **Basis / support skills**: `sap-basis-support:sap-troubleshooting`, `sap-log-reference`, `sap-health-triage`, `sap-transport-mgmt`, `sap-security-patch`, `sap-sscr-keys`, `sap-support-case`.
- **Data Volume Management**: `sap-dvm:dvm-analyse` (French, produces a Word report).

## Language

The user (SAP consultant) often works in French. Do not use em dashes or en dashes in replies, and say so when you do not know something rather than guessing.
