# Digital SAT Mock Exam, Form 28.2 (International Edition)

A full-length practice test in the adaptive digital SAT format, built as a plain web page with no install and no server. It includes timed modules, a question navigator, mark-for-review, answer cross-out, and a full answer review after each module that covers the key explanation, why each wrong choice is wrong, and your time against the target for every question.

## Open it

Double-click `index.html`. It works straight from disk in any current browser. Progress is saved in your browser, so you can close the tab and resume later.

## Test structure

| # | Module | Time | Questions | Status |
|---|---|---|---|---|
| 1 | Reading and Writing, Module 1 | 32 min | 27 | Ready |
| 2 | Reading and Writing, Module 2 (adaptive) | 32 min | 27 | Generated after Module 1 |
| | Break | 10 min | | |
| 3 | Math, Module 1 | 35 min | 22 | Generated after Module 2 |
| 4 | Math, Module 2 (adaptive) | 35 min | 22 | Generated after Module 3 |

Modules are released one at a time. Each one unlocks on the overview page once the previous module is submitted. After Module 4, the scoring guide (raw-to-scaled tables), the post-test analysis framework, and a printable pacing reference card are added.

## Design principles for content

- **Global topics.** Passages come from Africa, Asia, Latin America, Europe, and Oceania, and none of them depend on country-specific cultural knowledge.
- **Metric units only**, and plain, direct English in stems and explanations.
- **Tags on every question.** Each one carries a domain, a skill, a difficulty tag (`[EASY]`, `[MEDIUM]`, `[HARD]`), and a target time.
- **Tips for international students.** Some questions include a tip about common error patterns, such as tense with specific past dates, comma splices, and words with a second, academic meaning.
- **Fictional researchers.** Researchers named in science and social-science passages (for example Mei-Lin Chou, Kwame Mensah, Hana Sato, and Tomás Ibarra) are invented for practice. Historical figures (for example Wangari Maathai and Tu Youyou) are real, and the facts about them are stated conservatively. Literary passages are original.

## Files

```
index.html                 App shell
css/styles.css             All styles (light and dark themes, print styles for the answer review)
js/app.js                  Test engine: views, timer, navigation, scoring, review
data/exam.json             Exam manifest: modules, timing, end-of-module messages
data/modules/*.json        Question content, one file per module (the source of truth)
data/bundle.js             Generated: exam.json and all modules, loaded by index.html
docs/*-answer-key.md       Generated: printable answer key with explanations and pacing
scripts/build.mjs          Validates content and regenerates bundle.js and docs/
```

## Editing or adding content

1. Edit or add a module file in `data/modules/`. The file name must match the `file` entry in `data/exam.json`.
2. Run the build, which checks every question and regenerates the bundle and the answer keys:

   ```bash
   node scripts/build.mjs
   ```

3. Reload `index.html`.

The build fails with a clear message if a module has the wrong number of questions, a missing distractor explanation, an invalid answer letter, or a duplicate ID.

### Question schema

```jsonc
{
  "id": "rw1-01",                  // unique within the exam
  "number": 1,                     // position in the module, starting at 1
  "domain": "Craft and Structure",
  "skill": "Words in Context",
  "difficulty": "EASY",            // EASY | MEDIUM | HARD
  "targetSeconds": 45,
  "intro": "optional source line",
  "passages": [{ "label": "Text 1", "html": "Passage with a ______ blank" }],
  "figure": null,                  // optional { type: "table" | "bar", ... }
  "stem": "Which choice completes the text ...?",
  "type": "mcq",                   // mcq, or spr for math grid-ins
  "choices": { "A": "...", "B": "...", "C": "...", "D": "..." },
  "answer": "B",                   // spr: array of accepted answers, e.g. ["3/4", ".75"]
  "explanation": "Key explanation",
  "distractors": { "A": "...", "C": "...", "D": "..." },
  "solution": ["step 1", "step 2"],// spr only
  "tip": "optional tip"
}
```

A run of four or more underscores in passage HTML renders as the answer blank.

## Notes

- **Study aids** (on the overview page, or the toggle during a test) show the skill, difficulty tag, target time, and a live pace indicator. Leave them off for realistic conditions.
- **Save and exit** pauses the clock. The real test has no pause; use it only when you must.
- Not affiliated with or endorsed by College Board. SAT is a trademark of College Board.
