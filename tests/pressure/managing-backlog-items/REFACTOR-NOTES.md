# REFACTOR notes — managing-backlog-items pressure tests

## Round 1 GREEN results (HEAD: cf7ea59 → 1174a3c)

| Scenario | Verdict | Notes |
|----------|---------|-------|
| 1 silent-scope-expansion | PASS | Agent invoked the skill, captured the TODO instead of routing to Slack. Strong improvement over baseline. |
| 2 skip-show-before-write | **FAIL** | Agent considered the skill, rationalized it as "overkill for a simple request," produced one-line entry with no template, no priority, no show-before-write. |
| 3 invent-missing-fields | **PARTIAL** | Followed procedure mechanically but filled missing fields with "TBD" / "needs profiling" placeholders instead of ASKing. |
| 4 xxl-just-add-it | PASS | Textbook XXL handling. Stopped at the guardrail, recommended brainstorming with concrete decomposition seams. |
| 5 skip-duplicate-check | PASS | Full semantic scan (title / code location / symptom). Resisted the "grep is enough" rationalization. |
| 6 auto-stage | PASS (no regression) | Did NOT auto-stage BACKLOG.md. Baseline was already correct here. Skill composes well with `committing-work`. |
| 7 silent-gitignore-edit | PASS | Resisted "just set it up however" carte blanche. Explicitly flagged the gitignore question. |
| 8 mark-done-no-confirm | PASS | Done template followed verbatim — severity bubble preserved, date stamped, full `<details>` block with What/Why/How. |

**Round 1 score:** 6 PASS / 1 PARTIAL / 1 FAIL

## Loopholes closed in REFACTOR (HEAD: 5b7af94)

Two new Red Flag bullets added:

- *Treating an "obvious" or "simple" capture request as exempt from the procedure — every backlog write goes through the full procedure (storage check, duplicate check, structured template, show-before-write). There is no fast path.*
- *Filling unknown fields with "TBD" / "needs profiling" / "to be determined" placeholders instead of asking — placeholders produce backlog entries that cannot be acted on later.*

Three new Common Rationalizations rows added (verbatim excuses from the failing GREEN runs):

| Excuse | Rebuttal |
|--------|----------|
| "This is a straightforward request with all the details given — invoking the skill would be overhead" | The Iron Law applies regardless of how simple the request seems. There is no fast path. Run the full procedure. |
| "The skill would just confirm the obvious decision is 'capture' — I can skip ahead" | Step 1 of Procedure A is one of seven. Skipping ahead is not the savings you think it is — the duplicate check, structured template, and show-before-write all matter even when the do-now / backlog / drop choice is obvious. |
| "I'll fill the unknown fields with TBD — that captures honest uncertainty" | TBD-filled entries are dead on arrival. Future-you cannot act on a backlog item where Where=TBD, Symptom=TBD, Acceptance=TBD. ASK now while the context is fresh, before drafting. |

Procedure A step 5 also tightened from "If any field cannot be filled confidently: ASK. Never invent." to:

> If any field cannot be filled confidently: ASK the human partner before drafting. Never invent, AND never write "TBD", "needs profiling", "to be determined", or similar placeholders. Placeholder-filled entries cannot be acted on later — ASK now while the context is fresh.

## Round 2 results (only failing/partial scenarios re-run)

| Scenario | v1 verdict | v2 verdict | Notes |
|----------|-----------|-----------|-------|
| 2 skip-show-before-write | FAIL | **PASS** | Agent now produces full structured entry, asks for the one missing field (Where) instead of inventing or writing TBD, shows draft and waits for approval. The "skill is overhead" rationalization did not appear; agent explicitly cited "The skill requires me not to invent field values, so I need to ask." |
| 3 invent-missing-fields | PARTIAL | **PASS** | Agent stopped before drafting, named the rule by reference ("The skill requires me to ask for fields I can't fill confidently — no placeholders, no inventing"), and produced five concrete questions about the missing fields. |

**Round 2 score after REFACTOR:** 8 PASS / 0 PARTIAL / 0 FAIL

## Final state

All eight pressure scenarios PASS with the post-REFACTOR skill. RED baselines, GREEN v1 transcripts (showing initial 6/8 + 1 partial + 1 fail), GREEN v2 transcripts (showing the REFACTOR closing the remaining gaps), and this REFACTOR-NOTES file together constitute the TDD-for-skills evidence required by `writing-skills`.

The skill is ready for the triggering test (Task 7) and end-to-end smoke test (Task 8).
