# Caliber SAT

A complete SAT preparation app for **Caliber Learning Center** (Tashkent). It's one self-contained HTML page: Bluebook-style practice tests, an automatic diagnostic report, 38 interactive topic lessons, a six-month study plan, and links to the center's books online.

No server, no accounts, no install. Open the page on a phone or a laptop and start.

---

## What's inside

| Part | What it does |
| --- | --- |
| **Home** | Welcome page for students and parents: how the program works, plus three interactive charts (SAT area share vs. practice volume, practice per topic, starting point by score) |
| **Practice tests** | 3 original full tests (98 questions each, 294 total) in the Digital SAT format: 2 Reading & Writing modules and 2 Math modules, with timing |
| **Bluebook-style runner** | Timer you can hide, Directions, highlighter (4 colours + underline), ABC cross-out, Mark for Review, question navigator, review page, graphing calculator, reference sheet |
| **Report** | Level estimate per section, score by domain, weak topics ranked by priority, mistake habits (trap codes), timing analysis, and a full answer review with explanations |
| **Teacher check** | Students send a short result code (`CLC1.…`); the teacher pastes it and sees the same report. Paper answer sheets can be typed in too |
| **Topic lessons** | All 38 SAT question types, taught one by one: method, a worked example revealed step by step, a "your turn" question with instant feedback, traps, and 4 practice steps |
| **Study plan** | 6 months × 12 lessons for English and for Math (144 lessons), each with exact homework and tickable progress |
| **Books** | 9 center books linked to Google Drive, plus College Panda SAT Math chapter mapping. Every reference gives **book · pages · question numbers · answer-key page** |

## Screenshots

### Home
<img width="2910" height="1576" alt="Dia230412@2x" src="https://github.com/user-attachments/assets/124c634c-2b3c-420a-b244-700e49de0087" />


### Practice test (Bluebook-style)
<img width="2938" height="1846" alt="Dia230615@2x" src="https://github.com/user-attachments/assets/587a2000-1fda-41a0-9e47-522b09a6f8cc" />


### Math question with calculator
<img width="2938" height="1842" alt="Dia230543@2x" src="https://github.com/user-attachments/assets/e14ebea0-c2fc-46a8-a2eb-bdeeef01a274" />


### Student report
<img width="2384" height="1640" alt="Dia230727@2x" src="https://github.com/user-attachments/assets/e9b5b8a6-7f08-402f-8ec6-20f7983d351f" />


### Topic lesson
<img width="2940" height="1848" alt="Dia230826@2x" src="https://github.com/user-attachments/assets/5684de21-5ffd-4b24-8c8f-fc08dbba5d4b" />
<img width="2940" height="1848" alt="Dia230852@2x" src="https://github.com/user-attachments/assets/0b1ad35d-4231-47e9-a257-8a5c88f3dbb3" />
<img width="2940" height="1848" alt="Dia230930@2x" src="https://github.com/user-attachments/assets/3564d68d-0cc4-40df-9da7-77ef028d0d9b" />
<img width="2940" height="1848" alt="Dia231004@2x" src="https://github.com/user-attachments/assets/dbb8264b-9a99-4d4c-8633-fc3871cd5031" />



### Study plan
<img width="2424" height="1742" alt="Dia231055@2x" src="https://github.com/user-attachments/assets/a7d2ac40-054d-4f44-89a8-2063c4b53234" />


### Mobile
<img width="630" height="1280" alt="IMAGE 2026-10-28 23:13:06" src="https://github.com/user-attachments/assets/dac939d9-130a-4583-8990-971b5add23bc" />


## Highlights

- **Real math typesetting.** MathJax renders stacked fractions, roots and exponents the way the SAT shows them.
- **Fonts:** Noto Serif for test content (Bluebook-like), Lexend for reading the app, Space Grotesk for big titles only.
- **Honest scoring.** Scores are a level estimate for placement, not an official SAT score.
- **Checked answers.** Every math answer was verified in code (SymPy). Answer letters are balanced, and Reading options are checked so the correct answer isn't simply the longest one.
- **Progress stays on the device.** Test progress, highlights, lesson steps and plan ticks are saved in `localStorage`. Old result codes still open (as Practice Test 1).
- **Works on mobile.** Tested at 390 px and 1280 px wide with no horizontal scroll.

## Project structure

```
build/
├── caliber-sat.html      # the finished app (single file, all data inline)
├── app3_template.html    # page template: layout, styles, app logic
├── welcome.css / .js     # Home page styles and interactive charts
├── calc_block2.js        # graphing calculator + reference sheet
├── rw.py                 # Practice Test 1 – Reading & Writing (54 Q)
├── rw_bc.py              # Practice Tests 2 & 3 – Reading & Writing
├── math.py / math_tex.py # Math questions (TeX) for all three tests
├── guides.py             # 38 topic guides, trap codes, book list
├── lessons_rw.py         # lesson content for R&W topics
├── lessons_m.py          # lesson content for Math topics
├── roadmap.py            # six-month study plans
├── bookref.py            # turns "RB Inference Q1-50" into full page/question references
├── pagemap.json          # question → page map for each book topic
├── stats.py              # real numbers behind the Home page charts
├── build2.py             # builds data2.json (forms, lessons, plan, books)
└── assemble.py           # injects data + scripts into the template → caliber-sat.html
```

## Build

Requires Python 3.

```bash
cd build
python3 build2.py      # questions, lessons, plan and books → data2.json
python3 assemble.py    # template + data → caliber-sat.html
```

Open `caliber-sat.html` in any modern browser.

**External scripts** (loaded from jsDelivr, pinned versions):
- `mathjax@3.2.2` – math rendering
- `mathjs@11.12.0` – graphing calculator

Fonts come from Google Fonts (Lexend, Noto Serif, Space Grotesk).

### Testing

End-to-end tests use Playwright. They route MathJax and math.js to local `node_modules` copies:

```bash
npm i mathjax@3.2.2 mathjs@11.12.0 playwright
```

They check that each test scores 1600 with all answers right, that the result code round-trips through Teacher check, that highlighting works, and that pages don't overflow at mobile width.

## Editing content

- **Add or fix a question:** edit `rw_bc.py` / `math_tex.py`, then rebuild. Each wrong option carries a trap code (`EXT`, `OUT`, `SVA`, `CALC`…) that feeds the report's "mistake habits".
- **Change homework:** edit `roadmap.py` or the `ladder` in `guides.py` using short refs like `Boundaries Q26-50` or `SATakror #13, 36`. `bookref.py` expands them into full references with pages.
- **Don't reorder Practice Test 1.** Its question order is frozen so old result codes keep working.

## Notes

- The Drive book files must be shared as **"Anyone with the link can view"** for students to open them.
- College Panda is a commercial textbook. The app only references its chapters and pages and doesn't host the book.
- All test questions are original, written in the Digital SAT format. Book homework uses the center's own collections.

## Contact

**Caliber Learning Center** · Tashkent
+998 (95) 756-0-755 · @CALIBER_LEARNING_CENTER
