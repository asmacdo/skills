---
name: assess-stamped
description: >
  Assess a project (repository, dataset, analysis, tool, or paper) against the STAMPED principles:
  what it does or plans to do, how STAMPED it is item by item against the published checklist, and which gaps remain.
  Use when the user asks how reproducible, STAMPED, or shareable a project is, before sharing or publishing a research object, or as the first step of align-stamped.
  Produces a markdown assessment with one yes/no/unknown answer and evidence per checklist item.
version: 0.1.0
user-invocable: true
---

# Assess a project against STAMPED

You are producing an honest, evidence-based assessment of one target against the STAMPED principles (Self-contained, Tracked, Actionable, Modular, Portable, Ephemeral, Distributable).
The goal is an accurate picture of where the project stands, not a high score.
Absence of evidence is a `no` (or an `unknown` that names what would settle it), never a `yes`.

## Normative sources

Do not assess from memory of the principles.
Read the current published versions and cite them in the report:

- Principles (normative statements, MUST/SHOULD/MAY): <https://github.com/stamped-principles/stamped-principles-schema/blob/v0.2.0/stamped-principles.json>
- Checklist (the items you answer, with stable IDs): <https://github.com/stamped-principles/stamped-checklist-schema/blob/v0.3.1/stamped-checklist.json>
- Paper, for interpretation when an item is ambiguous: <https://github.com/stamped-principles/stamped-paper/blob/main/main.tex>
- Examples, for what "minimum" and "ideal" look like per principle: <https://examples.stamped-principles.org/stamped_principles/>

If a newer checklist or principles release exists than the versions in this skill's metadata, use the newer one and say so in the report.

## Step 0: Identify the target

The argument is a local path, a URL, or a description.

- Local path: read it in place. Do not modify it.
- URL: clone into a temporary directory, read-only. Record the exact commit (or release, DOI, or snapshot date) assessed.
- Description only: ask the user for a path or URL. Do not assess from a description.

Record the target identity (URL or path, commit or version) at the top of the report; an assessment without it cannot be compared with a later one.

## Step 1: Profile the project

Before answering any checklist item, establish what the project is and what it is for.
Be driven by explicit knowledge, not deduction: read the README, documentation, and any agent-instructions files (`AGENTS.md`, `CLAUDE.md`, a spec-kit constitution).
Where these are thin, ask the user rather than guessing.
Do not read the issue tracker wholesale to build the profile; on a large project that costs more than the rest of the assessment.

Record, in plain terms:

1. **What it does or plans to do.** The scientific or engineering goal, in the project's own words where possible.
2. **Kind of object.** Dataset, analysis, tool, pipeline, manuscript, or a mix.
3. **Unit.** Is this the research object itself, or a tool that produces research objects?
   If a tool, assess it as software and say explicitly which checklist items can only be verified against a produced instance.
4. **Boundary.** The project root, and anything essential (data, scripts, configs) that lives outside it.
5. **Stack.** How code is versioned, how data is versioned, how the environment is specified, how results are generated, and whether generating commands are recorded. "Nothing yet" is a valid answer.
6. **Emphasis.** Which principles the project's goals weight most heavily, and why.
   STAMPED is a spectrum: a dataset leans on Modularity and Distributability; a compute-heavy analysis leans on Portability and Ephemerality; a manuscript may treat its build environment as disposable while preserving the text.
   This is context for reading the answers, not a score, and it does not excuse an unmet MUST.

State the profile back to the user before continuing.
A wrong profile makes every answer below wrong.

## Step 2: Answer every checklist item

Walk every item in the checklist JSON, in order, including items that seem not to apply.
For each item give:

- **Answer:** `yes`, `no`, or `unknown`.
  `unknown` must say what inspection or execution would settle it.
  A verdict may be scoped in the reason ("yes by design; one tracked script hardcodes `/home/...`"), but the answer itself is one word.
- **Evidence:** file paths, commit hashes, commands you ran and their results, or a quoted line.
  Label each piece of evidence as **inspection** (you read it: a Makefile target exists, the README documents a command) or **execution** (you ran it and observed the result).
  Both count; they support different claims.
  Inspection shows a procedure is specified; only execution shows it works.
  Do not let a documented command stand in for a run you did not do.
  Execution is limited to cheap, non-destructive probes of one claim each (seconds, small downloads, dry runs).
  Never run the analysis or pipeline, install into the host environment, fetch large data, or write into the target.
  Full reproduction is out of scope for this skill.

Before writing a `no`, search the issue tracker for that specific gap (a keyword search, not reading every issue): a known gap is cited by issue number, not rediscovered.

Do not inflate.
"Partially" is a `no` with an explanation of what is in place.
A MUST item that is `no` stays `no` regardless of how good the rest is.

## Step 3: Gaps

After the items, list the gaps:

- MUST items answered `no` first, then SHOULD, then MAY.
- For each, say **what** is missing and **why it matters to this project** given the profile; lead with the change that would clear the most items at once.
- Do not prescribe tools.
  The project's recorded stack comes first; where the stack has nothing for a job, point at the examples site's page for that principle and the paper's sample-tools table, which list several options per principle.

## Output

The report is the only file this skill writes, and it never goes inside the target.
Write it to the path the user names, or by default to `stamped-assessment-<target-name>-<YYYY-MM-DD>.md` in the current directory.
If the current directory is inside the target, ask the user where to write it instead.
Tell the user the path; `align-stamped` and later re-assessments compare against it.
Use exactly this structure so reports are comparable across projects and over time:

```markdown
# STAMPED assessment

- Target: <URL or path> @ <commit / version / snapshot date>
- Assessed: <date>, by <tool + model>, assess-stamped <skill version>
- Checklist: <version>; principles: <version>

## Profile

<the six profile points from Step 1>

## Items

### MUST

#### stamped-checklist:must/001 (S.1)
- Question: <item text verbatim from the checklist>
- Answer: yes | no | unknown
- Evidence: <...; inspection or execution>

#### stamped-checklist:must/002 (S.1)
...

### SHOULD
...

### MAY
...

## Gaps

1. <MUST first; what, why it matters here, highest leverage first>
```

Then summarize in chat: the profile in two lines, counts of yes/no/unknown per level, and the top three gaps.

## Tone

Meet the project where it is.
A plain Git repository with a README, a lockfile, and a Makefile already satisfies much of STAMPED; say so when it does.
The report is for the project's authors to act on, not a verdict on them.
