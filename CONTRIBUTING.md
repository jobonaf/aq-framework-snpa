# Contributing

Repository: [https://github.com/jobonaf/aq-framework-snpa](https://github.com/jobonaf/aq-framework-snpa)

This document explains how to contribute to the repository in a simple and clear way.
It is intended for both people and LLM systems.

## Report a bug or issue

If you find an error, confusing content or an incorrect rule, open an issue.
You do not need to know how to fix the problem: it is enough to describe what you found.

- A clear report is already very useful.
- Always indicate the file or page concerned.
- If possible, add an excerpt of the text or a screenshot.

## How to open an issue

- Go to: [https://github.com/jobonaf/aq-framework-snpa/issues/new](https://github.com/jobonaf/aq-framework-snpa/issues/new)
- Write a short and specific title.
- Explain:
  - what you saw;
  - where you found it;
  - what you expected.
- If possible, add:
  - the file path (`docs/...`);
  - a text excerpt;
  - a screenshot.

### Example

- Title: Bug in `docs/modules/assess.md`
- Description: The threshold formula seems wrong; I expected an annual threshold, not a daily one.

## Repository structure

The documentation is organised as follows:

```text
docs/
  modules/          # module rules and logic
  tables/           # normative parameters and thresholds
  introduction.md
  architecture.md
  TODO.md
CONTRIBUTING.md
```

- **Modules** contain operational rules.
- **Tables** contain only normative data.
- Do not mix rules and data in the same file.

## Basic rules

- Descriptive text may be written in **Italian or English**.
- The repository is bilingual: Italian and English versions of descriptive documentation may coexist.
- When both language versions exist, keep them aligned in meaning and structure.
- Module, variable and function names: **English**.
- Requirement identifiers: **English**.
- EIONET vocabularies: **unchanged**.
- Always check whether a similar issue or requirement already exists.

## Requirements (REQ)

Each important rule is described with a REQ block.

Format:

```text
REQ-{MODULE}-{SLOT}
```

Where:

- `{MODULE}` is the module name (`ASSESS`, `REPR`, `NETWORK`, ...);
- `{SLOT}` is a short, descriptive and permanent word.

Examples:

- `REQ-ASSESS-CLASSIFICATION`
- `REQ-ASSESS-THRESHOLD_COUNT`
- `REQ-REPR-TOLERANCE_INTERVAL`
- `REQ-NETWORK-MIN_STATIONS`

Main rules:

- the identifier is permanent;
- do not rename existing requirements;
- do not use sequential numbers as logical ordering;
- new requirements must use a new descriptive identifier.

## Requirement format

Each rule must contain:

- identifier;
- source;
- status;
- type;
- dependencies;
- rule;
- acceptance criterion;
- pseudocode.

### Template

```markdown
##### REQ-

- Source: Art. X Dir. 2024/2881 [+ Annex Y]
- Status: STABLE / DRAFT / AMBIGUOUS / PENDING
- Type: mandatory / recommended / optional
- Dependencies: REQ-XXX-YYY, tables/ZZZ

**Rule**
Short and clear description of the rule.

**Acceptance criterion**
Given [condition], the system [expected behaviour].

**Pseudocode**
Descriptive, not executable.
```

## Notes for LLMs and reviewers

- The REQ block is the minimum unit.
- Do not split one requirement across multiple files.
- Do not change the source without checking it.
- For a `PENDING` requirement, do not complete the logic.
- Before adding a requirement, check that it does not already exist.
