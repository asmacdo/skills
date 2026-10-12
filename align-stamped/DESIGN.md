# align-stamped: design notes

Scope decisions for the first version and the work left for separate follow-ups.
`SKILL.md` stays lean because every line of it is loaded into the agent's context; the reasoning lives here.

## Scope

`align-stamped` is the intervention half of the pair proposed in stamped-principles/stamped-agent-skills#3; `assess-stamped` is the measurement half.
It changes a project's meta-architecture (agent-instruction files, constitution, skills) so the STAMPED principles hold in later sessions, and it proposes options for the gaps the assessment found.
It does not itself close the gaps; the user chooses, and the project's own work tracking holds the choice.

Out of scope here, tracked in that issue: creating new projects from scratch, ReproNim-specific variants, and full research-loop automation.

The skills live in con/skills first, to reuse its validation CI and conventions and to be used by the people writing them; whatever proves useful can be separated into stamped-principles/stamped-agent-skills later, possibly with an automatic sync.

## Decisions

1. **Assess first.** The values are baked in proportionate to the project's goals, and the profile that captures those goals is produced by `assess-stamped`.
   Writing the harness section before assessing would mean deducing the emphasis, which stamped-agent-skills#3 asks to avoid.
2. **Proportionate, not maximal.** STAMPED is a spectrum, and the harness section names only the principles and rules the project's goals call for.
   A section that lists every rule for every project is the one that gets skimmed and ignored.
3. **Harness-agnostic.** The section goes into whatever the project's tools already read (`AGENTS.md`, `CLAUDE.md`, a spec-kit constitution, Copilot instructions); the skill never adds a second file of the same kind and asks before creating a first one.
   This is the lowest common denominator across agent harnesses: a pointer line is the distribution mechanism when there is no skill-trigger mechanism.
4. **Pointer, not paste.** The harness section links to the normative sources rather than restating them, for the same drift reason recorded in `assess-stamped/DESIGN.md`.
5. **Options, not prescriptions.** Each gap gets two or three options with tradeoffs, the project's recorded stack first, then the examples site and the paper's sample-tools table.
   The user chooses; the chosen items go into the project's existing work tracking.
6. **Re-assess by offer.** The loop in stamped-agent-skills#3 (align, re-assess, align more) is closed by offering a re-assessment, not by running it unasked; the user controls the iteration.

## Deferred, each as a separate follow-up

- **`create-stamped-project`.** Alignment on an empty directory plus a background-gathering prelude (similar projects, the user's environment).
  This skill already handles the no-harness case so that a create skill can delegate to it.
- **ReproNim-specific variant.** A wrapper that prepares the prompt with ReproNim technologies; this skill stays generic.
- **Driving spec-kit's analysis.** When a constitution is updated, running spec-kit's own analysis to generate tasks is left to the user.
- **Suggesting other skills from this collection** (e.g. `introduce-reuse-compliance` for license gaps) as options for specific gaps.
  Worth doing once there is evidence which gaps recur; a hardcoded mapping now would be a prescription by another name.
- **Measuring the effect.** Whether aligning a project changes what later sessions do is the A/B question in stamped-agent-skills#3; `assess-stamped` run before and after is the instrument, and that experiment is separate from this skill.
