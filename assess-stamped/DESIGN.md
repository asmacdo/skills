# assess-stamped: design notes

Scope decisions for the first version, and the work deliberately left for separate, drop-in follow-ups.
`SKILL.md` stays lean because every line of it is loaded into the agent's context; the reasoning lives here.

## Scope

`assess-stamped` answers three questions about one target: what does it do (or plan to do), how STAMPED is it, and what gaps remain.
It reads, it does not modify.
It is the measurement half of the pair proposed in stamped-principles/stamped-agent-skills#3; `align-stamped` is the intervention half and calls this skill first and last.

Out of scope here, tracked in that issue: creating new projects, ReproNim-specific variants, and any full research-loop automation.

The skills live in con/skills first, to reuse its validation CI and conventions and to be used by the people writing them; whatever proves useful can be separated into stamped-principles/stamped-agent-skills later, possibly with an automatic sync.

## Decisions

1. **No condensed STAMPED context in the skill.** The skill links to the published principles JSON, checklist JSON, paper, and examples site, pinned by tag, and tells the agent to read them.
   Three earlier assessors each paraphrased the normative statements inline and each drifted from the published codes (`stamped-examples` `/stamped-assess`, the morphometrics `references/assessment.md`, and the July 2026 draft in stamped-principles/stamped-agent-skills).
   A condensed `stamped-context` that is generated from the JSON sources, so it cannot drift, is a separate optimization; until it exists, linking is the version that cannot be wrong.
2. **One answer per checklist item, keyed by the checklist's stable item ID, with evidence for every answer.** The checklist web UI stores `{value: yes | no | null, reason}` per item ID, but it only shows the reason field for a `no` ("Reason for not meeting requirement").
   An assessment needs the evidence behind a `yes` just as much, so the report is plain markdown rather than that schema, and is not blocked on changing it.
   `unknown` maps to `null`.
3. **Profile before items.** The checklist does not carry the project's goals, but the right reading of every answer depends on them.
   The profile's "emphasis" is the lightweight form of the `rubric` record in stamped-principles/stamped-skills-assessment (per-principle priority and direction); it is prose here so the two can converge later without this skill committing to a 0.1 schema.
4. **Evidence names its strength.** Inspection (a definition was read) and execution (it was run and observed) are reported as different things, following the distinction in stamped-skills-assessment's design.
5. **Gaps name what and why, never which tool.** Options are delegated to the examples site's per-principle pages and the paper's sample-tools table, both of which list several tools per principle.

## Deferred, each as a separate drop-in

These are not skipped because they are wrong; they are scoped out so this skill can land small and be measured, and each is shaped so it can be added without changing the report format above.

- **Structured output.** The report is markdown so a human can read and review it today.
  A converter from the fixed markdown structure to JSON keyed by item ID is a small script; the format in `SKILL.md` was fixed with that in mind.
- **Checklist share link.** The web UI's link is `checklist=<v>&responses_version=<v>&format=3&responses=<base64 JSON>` with `{checklist_version, responses: {<item-id>: {value, reason}}}` (reason truncated at 250 characters).
  Emitting it is the same converter plus one encoding step; the evidence behind `yes` answers would be dropped until the UI carries it.
- **stamped-skills-assessment record.** That repository's `assessment` record wants the same per-item judgments plus declared activities and observations; emitting it is an adapter over the structured output, to be written once that schema is past its pilot.
- **Hands-on validation phase.** Cloning, building, and executing the target to turn inspection evidence into execution evidence, as `/stamped-assess` in stamped-examples offers as its Phase 2.
  Costly and environment-dependent; a later opt-in step that only upgrades evidence strength, never the report structure.
- **FAIR.** The stamped-examples assessor also rates F, A, I, R. Kept out so this skill measures one thing.
- **Generated normative context (`stamped-context`).** See decision 1.
