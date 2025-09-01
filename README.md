# Prep: DSA + System Design

A clean, practical workspace for algorithmic problem solving and system design prep. Focus is on correctness, performance, and repeatable workflows.

## What’s Inside

- **DSA**: Roadmaps, canonical algorithm templates, and problem walkthroughs
  - [DSA/Algorithms/ROADMAP.md](DSA/Algorithms/ROADMAP.md)
  - [DSA/Algorithms/ALGORITHM_REFERENCE.md](DSA/Algorithms/ALGORITHM_REFERENCE.md)
- **Problems**: NeetCode 150 index + worked solutions using a strict template
  - [DSA/Problems/NEETCODE 150.md](DSA/Problems/NEETCODE%20150.md)
  - [DSA/Problems/PROBLEM_WALKTHROUGH.md](DSA/Problems/PROBLEM_WALKTHROUGH.md)
  - [DSA/Problems/NeetCode_Solutions/](DSA/Problems/NeetCode_Solutions/)
- **SystemDesign**: Patterns, case studies, roadmap, and index
  - [SystemDesign/SYSTEM_DESIGN_ROADMAP.md](SystemDesign/SYSTEM_DESIGN_ROADMAP.md)
  - [SystemDesign/INDEX.md](SystemDesign/INDEX.md)
  - Templates: [SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md](SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md), [SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md](SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md)

## How to Use This Repo

- **If you’re doing DSA:**
  1. Read [DSA/Algorithms/ROADMAP.md](DSA/Algorithms/ROADMAP.md) to choose the next topic.
  2. Document the concept using [DSA/Algorithms/ALGORITHM_REFERENCE.md](DSA/Algorithms/ALGORITHM_REFERENCE.md) (fill it properly: assumptions, proof sketch, pitfalls, variants, pseudocode).
  3. Grind problems from [DSA/Problems/NEETCODE 150.md](DSA/Problems/NEETCODE%20150.md).
  4. For each problem, strictly follow [DSA/Problems/PROBLEM_WALKTHROUGH.md](DSA/Problems/PROBLEM_WALKTHROUGH.md) (brute-force → redundancy → optimal → complexity) and store the write-up in [DSA/Problems/NeetCode_Solutions/](DSA/Problems/NeetCode_Solutions/).

- **If you’re doing System Design:**
  1. Follow [SystemDesign/SYSTEM_DESIGN_ROADMAP.md](SystemDesign/SYSTEM_DESIGN_ROADMAP.md).
  2. Use [SystemDesign/INDEX.md](SystemDesign/INDEX.md) to jump directly to topics/files.
  3. For patterns, use [SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md](SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md) (fill SLOs, tradeoffs, failure modes, capacity math—no hand-waving).
  4. For case studies, use [SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md](SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md) (requirements → capacity → APIs → data model → scaling → resilience → cost → testing).

## Zero-Excuse Study Loop

- **Algorithms**: For every technique you “learn,” produce a filled `ALGORITHM_REFERENCE.md` artifact. No artifact, no learning.
- **Problems**: Every problem gets a full walkthrough write-up. If you can’t justify the optimality or prove correctness, it’s not done.
- **Design**: Every pattern/case study must include: SLOs, failure handling, back-of-the-envelope math, tradeoffs table, and runbooks.

## Quick Links

- DSA
  - Roadmap: [DSA/Algorithms/ROADMAP.md](DSA/Algorithms/ROADMAP.md)
  - Template: [DSA/Algorithms/ALGORITHM_REFERENCE.md](DSA/Algorithms/ALGORITHM_REFERENCE.md)
  - Problem Template: [DSA/Problems/PROBLEM_WALKTHROUGH.md](DSA/Problems/PROBLEM_WALKTHROUGH.md)
  - Example solution: [DSA/Problems/NeetCode_Solutions/01_Contains_Duplicate.md](DSA/Problems/NeetCode_Solutions/01_Contains_Duplicate.md)
- System Design
  - Roadmap: [SystemDesign/SYSTEM_DESIGN_ROADMAP.md](SystemDesign/SYSTEM_DESIGN_ROADMAP.md)
  - Index (topic → file): [SystemDesign/INDEX.md](SystemDesign/INDEX.md)
  - Pattern Template: [SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md](SystemDesign/Patterns/SYSTEM_DESIGN_PATTERN.md)
  - Case Study Template: [SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md](SystemDesign/CaseStudies/CASE_STUDY_TEMPLATE.md)

## Conventions (keep it consistent)

- One topic per file. File names are descriptive, not cute.
- Prove correctness or give a solid proof sketch. Don’t just assert.
- Always include time/space complexity and edge-case checklists.
- Prefer canonical pseudocode first; then show a minimal, correct implementation.

## License

Educational use. If you extend it, keep the structure and templates intact so others can actually learn from it. 
