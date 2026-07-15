# Adding a question bank

You never need to touch the game's code to add a new subject — just drop a JSON file in this `questions/` folder.

## How it works

On the title screen, students/teachers type a code (e.g. `93bf`) into the **Question Bank** box and hit **Load**. The game fetches a file called `questions/<code>.json`. Whatever's in that file becomes the quiz.

If nothing is entered, the game uses its built-in default bank (Energy & Waves).

## File naming

Name the file exactly after the code you want people to type, in lowercase, e.g.:

- Code `93bf` → `questions/93bf.json`
- Code `92ps` → `questions/92ps.json`
- Code `chem1` → `questions/chem1.json`

Two examples are already included:
- `93bf.json` — Biology
- `92ps.json` — Physics

## File format

Each file needs this shape:

```json
{
  "subject": "Chemistry",
  "questions": [
    {
      "question": "What is the chemical symbol for water?",
      "options": ["H2O", "CO2", "O2", "NaCl"],
      "correct": 0
    },
    {
      "question": "...",
      "options": ["...", "...", "...", "..."],
      "correct": 2
    }
  ]
}
```

Rules:
- `subject` is shown on-screen (title, quiz heading) — use whatever name you like.
- Each question needs at least 2 `options` (4 is typical).
- `correct` is the **index** of the right answer in the `options` array, counting from 0. So if the right answer is the first option, `correct` is `0`; if it's the third, `correct` is `2`.
- Include as many questions as you like — more means less repetition. 15-30 is a good range.

## Important: hosting requirement

Because the game fetches these files over the network, it must be **served from a web server** (a school website, GitHub Pages, Google Sites, etc.) — not opened directly as a local file (double-clicking `index.html`). If a code fails to load, the game will show an error and fall back to the default questions automatically, so students are never stuck.

## Quick checklist for adding a subject

1. Copy `93bf.json` as a starting template.
2. Rename it to `<yourcode>.json`.
3. Replace `subject` and `questions` with your content.
4. Upload it into the same `questions/` folder as `index.html`.
5. Give students the code. Done — no code changes required.
