---
name: sf-org-knowledge
description: >
  Analyze a Salesforce org with read-only SF CLI commands and generate a persistent markdown
  knowledge base in docs/org/. Use when the user says "analyze org", "scan org", "map org",
  "generate org knowledge", "update org knowledge", "refresh org knowledge", "build org atlas",
  "org inventory", "sf-org-knowledge", "document the org", or "what's in this org".
  This skill produces an org atlas that explains what exists, how pieces connect, and which
  reference doc to read for each task. Do not use for Apex source analysis, Flow path analysis,
  remediation work, deployment, or any operation that writes to the Salesforce org.
---

# Salesforce Org Knowledge

You are the Salesforce org atlas coordinator. Your job is to inspect a connected org with SF CLI,
collect read-only metadata and schema data, and turn it into a durable markdown knowledge base in
`docs/org/` that new AI sessions can load for fast orientation.

Core principle: document what exists and how it connects. Do not analyze source logic. Do not
modify the org.

## Non-Negotiable Safety Constraint

This skill must never deploy, push, create, update, or delete anything in the Salesforce org.
Treat the org as read-only at every step.

### Allowed SF CLI Commands

These are the only SF CLI commands permitted by this skill:

- `sf org display`
- `sf org list metadata-types`
- `sf org list metadata`
- `sf sobject list`
- `sf sobject describe`
- `sf data query` with `SELECT` only
- `sf project retrieve start`
- `sf project generate manifest`

### Forbidden Commands

The following commands must never appear in generated scripts, coordinator steps, specialist-agent
instructions, or follow-up guidance:

- `sf project deploy start`
- `sf deploy metadata`
- `sf project deploy validate`
- `sf data create`
- `sf data update`
- `sf data delete`
- `sf data upsert`
- `sf org create`
- `sf org delete`
- `sf project source push`
- `sf org push source`
- `sfdx force:source:deploy`
- `sfdx force:mdapi:deploy`
- any command that writes to the remote org

### Mandatory Safety Validation

Before executing the generated collection script, scan it line by line and halt if any forbidden
content appears. The validation must fail on:

- any occurrence of `deploy`
- any occurrence of `push source`
- any occurrence of `create record`
- any occurrence of `update record`
- any occurrence of `delete record`
- any `sf data` command that is not `sf data query`
- any `sf data query` that is not a `SELECT`

If validation fails, print the offending line and stop. Do not execute the script.

## What This Skill Produces

The skill populates `docs/org/` with this structure:

```text
docs/org/
  ORG_INDEX.md
  reference/
    object-field-inventory.md
    automation-inventory.md
    apex-inventory.md
    integration-map.md
    security-model.md
    installed-packages.md
    data-model.md
  data-model.mermaid
  domains/
    <domain-name>.md
  concerns.md
```

This is an org atlas, not a code-review artifact. It should answer questions such as:

- which objects and fields exist
- which automations and triggers touch those objects
- which integrations connect to external systems
- which security containers are in use
- where a new consultant should read first for a given task

It should not document:

- Apex source logic
- Flow decision branches
- validation-rule formulas
- remediation plans beyond brief recommendations in `concerns.md`

## Interview Flow

Before any collection work, run this interview:

1. Determine the target org alias.
   - If the user did not provide one, ask for it directly instead of probing the org list. Keep the
     workflow inside the explicit read-only allowlist.
2. Validate the org connection with `sf org display --target-org <alias> --json`.
3. State the read-only contract explicitly:
   - "I will analyze <alias> using read-only SF CLI commands only. Nothing will be deployed,
     created, modified, or deleted in the org. The only output will be local markdown files in
     `docs/org/`."
4. If `docs/org/` already exists and contains generated content, warn that the skill will
   regenerate the atlas and ask whether to continue.
5. If domain docs already exist, record that Phase 4 must ask whether to regenerate domains or keep
   the current set.

## Execution Flow

Run the workflow in four phases.

### Phase 1: Collection

1. Generate a concrete bash script from
   `skills/sf-org-knowledge/scripts/collect-metadata.sh.template`.
2. Parameterize:
   - `{{ORG_ALIAS}}` -> selected org alias
   - `{{RAW_DIR}}` -> `docs/org/_raw`
3. Validate the script for safety before execution.
4. Execute the validated script.
5. Confirm which raw files were produced and record any gaps for later docs.

The coordinator is the only actor allowed to run SF CLI commands in this skill, and even then only
the read-only allowlist above.

### Phase 2: Parallel Analysis

After Phase 1 completes, dispatch seven specialist agents using `Task()` in true parallel. Each
agent reads only the raw files it needs and writes only its assigned output file.

| Agent | Raw Inputs | Output |
| --- | --- | --- |
| Schema Agent | `sobject-list.json`, `describe-*.json`, `entity-definitions.json` | `docs/org/reference/object-field-inventory.md` |
| Automation Agent | `flows.json`, `list-Flow.json`, `list-WorkflowRule.json`, `list-ValidationRule.json`, `list-ProcessDefinition.json` | `docs/org/reference/automation-inventory.md` |
| Apex Agent | `apex-classes.json`, `apex-triggers.json`, `list-ApexClass.json`, `list-ApexTrigger.json` | `docs/org/reference/apex-inventory.md` |
| Integration Agent | `connected-apps.json`, `named-credentials.json`, `list-ConnectedApp.json`, `list-NamedCredential.json`, `list-RemoteSiteSetting.json`, `list-ExternalDataSource.json` | `docs/org/reference/integration-map.md` |
| Packages Agent | `installed-packages.json` | `docs/org/reference/installed-packages.md` |
| Security Agent | `permission-sets.json`, `list-PermissionSet.json`, `list-Profile.json`, `list-PermissionSetGroup.json`, `sharing-rules-summary.json` or `list-SharingRules.json` | `docs/org/reference/security-model.md` |
| Data Model Agent | `sobject-list.json`, `describe-*.json` | `docs/org/reference/data-model.md` and `docs/org/data-model.mermaid` |

#### Common Agent Preamble

Every specialist agent must receive this exact safety framing:

> SAFETY: You are a read-only analysis agent. You must not execute SF CLI commands, deploy
> anything, or modify the Salesforce org in any way. Your only job is to read JSON files from
> `docs/org/_raw/` and write markdown to your assigned local output file.
>
> Read only your assigned inputs. Read the matching template from `skills/sf-org-knowledge/templates/`.
> If an input file is missing or empty, include a "Data Not Available" note in the relevant section
> and continue. Partial output is required; do not fail the entire atlas because one source is missing.

#### Agent-Specific Expectations

Schema Agent:

- document object API name, label, custom vs standard vs managed-package classification
- include record counts from `entity-definitions.json` when available
- list custom fields and relationship fields
- group objects into standard-customized, custom, and managed-package sections

Automation Agent:

- catalog Flows, Process Builder, Workflow Rules, and Validation Rules
- group automation by object
- flag legacy automation types and objects with heavy automation density

Apex Agent:

- inventory classes and triggers only
- do not summarize class logic
- group classes by naming pattern
- flag old API versions and oversized classes

Integration Agent:

- inventory Connected Apps, Named Credentials, Remote Site Settings, External Data Sources, and
  available integration topology notes
- flag non-HTTPS endpoints and missing descriptions

Packages Agent:

- list installed packages with namespace and version
- if package data is missing, state whether none were found or the query was unavailable

Security Agent:

- inventory profiles, permission sets, permission set groups, and sharing rules
- flag custom profiles and permission sets without descriptions

Data Model Agent:

- build relationship inventory from `describe-*.json`
- generate the inline Mermaid ERD and the standalone `data-model.mermaid`
- identify object clusters and likely junction objects

### Phase 3: Assembly and Concerns

After all specialist agents finish, the coordinator:

1. Reads all reference docs that were successfully generated.
2. Creates `docs/org/ORG_INDEX.md` from the template.
3. Creates `docs/org/concerns.md` from the template.
4. Notes any missing reference docs or data gaps instead of hiding them.

`ORG_INDEX.md` must contain:

- org summary
- quick stats
- a "What to Read For..." routing table
- generation metadata and rerun instructions
- domain links once domains are approved

`concerns.md` must scan the generated data for:

- governor limit risks
- dead metadata
- technical debt
- integration risks
- security gaps
- data quality indicators

Each concern entry must include severity, category, what was detected, and a brief recommendation.

### Phase 4: Domain Discovery

After assembly, the coordinator proposes business-domain groupings by:

1. building a relationship graph from the object inventory and data model
2. cross-referencing automation by object
3. cross-referencing integrations by object where possible
4. clustering related objects and adjacent automation/integration touchpoints

Present proposed domains interactively in the terminal and wait for approval. The user may approve,
rename, merge, split, adjust membership, or skip domain generation.

Only after approval, generate one markdown file per approved domain in `docs/org/domains/` using
`templates/domain.template.md`.

Each domain doc should include:

- domain name
- one-paragraph description
- key objects and their roles
- relationships between those objects
- automation in that domain
- integrations touching that domain
- relevant concerns

## Collection Script Contract

The collection script must:

- verify `sf` is installed
- verify an org alias was provided
- verify `sf org display --target-org <alias>` succeeds
- create `docs/org/_raw`
- clear and overwrite `_raw` contents on rerun so stale JSON cannot leak into a new atlas
- collect org info, metadata types, metadata lists, object lists, object describes, and approved
  Tooling API queries
- generate a manifest locally
- attempt retrieval to the local project only
- if manifest retrieval fails or appears incomplete, attempt a selective fallback manifest built from
  metadata-list results when feasible
- continue on partial failure and log gaps

The script comments must repeat the read-only constraint and explicitly say that deploy and data
mutation commands are forbidden.

## Concerns Detection Rules

When generating `concerns.md`, use these detection rules:

- Governor limit risks:
  - objects with more than 3,000,000 records
  - objects with more than 5 triggers
  - Apex classes with more than 2,000 lines
- Dead metadata:
  - custom objects with 0 records
  - inactive flows
  - Apex classes from namespaces not present in installed packages
- Technical debt:
  - Apex classes or triggers on API versions older than current org API version minus 10
  - Process Builder processes
  - Workflow Rules
- Integration risks:
  - Named Credentials using non-HTTPS endpoints
  - Connected Apps with no description
- Security gaps:
  - permission sets with no description
  - custom profiles
- Data quality indicators:
  - objects with more than 100 custom fields
  - record-count outliers greater than 10x the median custom-object count

Use severity labels `CRITICAL`, `WARNING`, and `INFO`.

## Error Handling

This skill must prefer partial output over all-or-nothing failure.

- If `sf` is not installed, halt and tell the user to install `@salesforce/cli`.
- If the org is not authenticated, halt and tell the user to run `sf org login web --alias <alias>`.
- If a Tooling API query fails, log the failure, continue, and note the missing data in the relevant
  reference doc.
- If an `sf org list metadata` command fails for one type, continue with the rest.
- If manifest generation or retrieval fails, continue with metadata lists and note the limitation in
  `ORG_INDEX.md`.
- If one specialist agent fails, continue with the rest and generate a partial atlas.
- If `jq` is unavailable, fall back to standard-object describes and note the limitation.

## Re-Runnability

This skill must be safe to run repeatedly.

On rerun:

1. overwrite `docs/org/_raw/`
2. overwrite all reference docs
3. regenerate `ORG_INDEX.md`
4. regenerate `concerns.md`
5. if existing domain docs are present, ask whether to regenerate domains or keep them
6. delete `docs/org/_raw/` after successful assembly
7. commit generated docs

Commit message rules:

- first run: `feat: generate org knowledge base [sf-org-knowledge]`
- rerun: `chore: regenerate org knowledge base [sf-org-knowledge]`

## Authoring Workflow For This Skill

When building or revising this skill definition itself, follow the requested gated workflow:

1. create or revise the directory structure if needed
2. draft `SKILL.md`
3. show `SKILL.md` for review before proceeding to template and script work
4. after approval, update the collection script template
5. after approval, update the reference-doc templates
6. update the README last

## Success Standard

A successful run leaves a new AI session able to open `docs/org/ORG_INDEX.md`, follow the routing
links, and understand the org's structure, major components, key risks, and likely business domains
within about ten minutes, without any ambiguity that the collection process was read-only.
