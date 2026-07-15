# Adding a question bank

A valid question bank code is **required** to play — there's no default quiz anymore. The **Begin Game** button stays disabled until a valid code is loaded.

Students and teachers enter a code into the single code box on the title screen and hit **Apply**. That box is used for two things:

1. **Question bank codes** (e.g. `93bf`) — loads a subject's quiz questions and unlocks Begin Game.
2. **Developer testing codes** (`wave`, `alien`, `powerup`) — shortcuts for testing the game (see below). These do NOT unlock Begin Game on their own — a real question bank code still needs to be loaded too.

Whatever's typed in is checked against the testing codes first; if it's not one of those, it's looked up as a question bank code.

## How question bank codes work

Near the top of the `<script>` in `index.html` there's a lookup table:

```js
const QUESTION_BANK_FILES = {
    '93bf': 'question-banks/93bf.json',
    '92ps': 'question-banks/92ps.json',
};
```

To add a new subject:

1. Create a new JSON file in the `question-banks/` folder (copy `93bf.json` as a starting template).
2. Add one line to `QUESTION_BANK_FILES` mapping your chosen code to that file's path.
3. Give students the code.

This is the only code change ever needed — you're not touching any game logic, just adding a line to this table.

## JSON file format

```json
{
  "subject": "Chemistry",
  "questions": [
    {
      "question": "What is the chemical symbol for water?",
      "options": ["H2O", "CO2", "O2", "NaCl"],
      "correct": 0
    }
  ]
}
```

- `subject` is shown on-screen (title, quiz heading).
- Each question needs at least 2 `options` (4 is typical).
- `correct` is the **index** of the right answer, counting from 0.
- 15-30 questions is a good range so quizzes don't repeat too often.

## Developer testing codes

Typed into the same code box:

| Code | Effect |
|---|---|
| `wave` | Shows a number picker to jump straight to a chosen wave (1-50), with 200 ammo |
| `alien` | Skips quizzes entirely, starts with 500 ammo |
| `powerup` | Skips quizzes, starts with 2000 ammo, and spawns a powerup every wave |

These don't touch the question bank — they're just gameplay shortcuts for testing.

## Hosting requirement

Because question bank files are fetched over the network, the game needs to be **served from a web server** (school website, GitHub Pages, etc.) — not opened directly by double-clicking `index.html`. If a code can't be loaded, the game shows an error explaining this, since there's no default fallback quiz anymore.

## Troubleshooting: "Couldn't load code" error

This almost always means `fetch()` is failing, which happens when:

- **The page was opened as a local file** (`file:///...`) instead of served over http(s). Browsers block JavaScript from reading local files for security reasons — this is the most common cause. Fix: host the folder on GitHub Pages, a school website, or run a simple local server (e.g. `python3 -m http.server` from the folder, then visit `http://localhost:8000`).
- **The `question-banks/` folder wasn't uploaded** alongside `index.html`, or was renamed. The folder must sit next to `index.html`, not inside a different directory.
- **The code isn't in `QUESTION_BANK_FILES`** in `index.html`, or is misspelled.

Open the browser's developer console (F12 → Console tab) for the exact error — it's logged there even though the on-screen message stays generic.
