# Interview Prep — Study Plan

## Structure

- `topics/1-oop.md` … `topics/10-logical-reasoning.md` — numbered, ordered by study sequence (Programming → Data Structures & Algorithms → CS Fundamentals → Logical Reasoning). Each file is the single source of truth for that topic.
- `study-plan.md` — day-by-day schedule.
- `practice-mcqs.md` — practice questions by section.
- `final-checklist.md` — pre-test checklist, one item per topic area.

## Workflow

1. Work through `topics/` in numeric order, one topic per session.
2. While studying a topic, freely enhance its md file — add examples, correct/expand notes, add gotchas hit while practicing. Each file stays self-contained; don't duplicate content across topics.
3. On finishing a topic, check off its line(s) in `final-checklist.md`.
4. Numbering must stay contiguous (no gaps, no duplicates) — if a topic file is split or merged, renumber the rest of `topics/` to match.

## Notes for Claude

- Don't add "WellDev" / "SWE" / employer-identifying terms to filenames or content — user is keeping this repo generic (see prior scrubbing).
- Don't create new topic files unless asked — enhancing an existing one is the default action when the user says they're studying a topic.
- When the user shares a reference link for a topic (MCQ bank, article, video, etc.), add it to that topic's `topics/N-name.md` file under a `## Further Practice` (or `## References` if not practice questions) section — don't ask, just add it.
