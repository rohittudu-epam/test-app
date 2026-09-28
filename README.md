# Quiz CLI

An interactive command-line quiz game for learning and reviewing JavaScript, Node.js, and general programming concepts.

## Overview

Quiz CLI is a dependency-free Node.js command-line application. It loads multiple-choice questions from `data/questions.json`, lets the player choose a category and quiz length, provides immediate answer feedback, and displays a final score with a review of incorrect answers.

The project uses native Node.js ES modules and requires Node.js 18 or newer.

## Features

- Interactive terminal gameplay using numbered menus and prompts.
- Three question categories: JavaScript Basics, Node.js Fundamentals, and General Programming.
- Quiz lengths of all available questions, 3 questions, or 5 questions when available.
- Fisher–Yates question shuffling.
- Progress display and visual progress bar.
- Immediate correct/incorrect feedback and optional explanations.
- Final percentage, performance message, and incorrect-answer review.
- Replay support.
- Local ANSI terminal color utilities without external dependencies.

## Prerequisites

- Node.js `>=18.0.0`
- npm, included with Node.js

## Installation

From the project root, run:

```bash
npm install
```

No third-party runtime or development dependencies are declared.

## Running the Quiz

```bash
npm start
```

The equivalent direct command is:

```bash
node index.js
```

## Gameplay Flow

1. Questions are loaded from `data/questions.json`.
2. Choose a category.
3. Choose all questions, 3 questions, or 5 questions when available.
4. Press Enter to begin.
5. Select answers from numbered choices.
6. Review feedback and explanations.
7. View the final score and incorrect-answer review.
8. Choose whether to play again.

Questions are shuffled before the requested number is selected, so shorter quizzes contain a randomized subset of the category.

## Question Data

Questions are stored in `data/questions.json` under a top-level `categories` object. Each category has a display `name` and `questions` array. Each question has question text, an options array, a zero-based answer index, and an explanation:

```json
{
  "categories": {
    "category-id": {
      "name": "Category display name",
      "questions": [
        {
          "question": "Question text",
          "options": ["Option A", "Option B", "Option C"],
          "answer": 0,
          "explanation": "Why the answer is correct."
        }
      ]
    }
  }
}
```

The supplied data contains three categories with five questions each, for 15 total questions. Keep `answer` aligned with the correct zero-based option index when customizing the data.

## Project Structure

```text
.
├── data/
│   └── questions.json   # Question data
├── src/
│   ├── colors.js        # ANSI color and style helpers
│   ├── input.js         # Readline prompts and selection helpers
│   └── quiz.js          # Quiz state, scoring, and results
├── index.js              # Application entry point
├── package.json          # npm metadata and scripts
└── README.md             # Project documentation
```

`.DS_Store`, if present, is operating-system metadata and is not application logic.

## Architecture and Module Responsibilities

- **`index.js`** resolves and loads the question file, renders menus, manages replay, coordinates the quiz and input modules, handles errors, and closes the readline interface.
- **`src/input.js`** creates the Node.js `readline` interface and provides Promise-based prompts, numbered selection, yes/no confirmation, and Enter pauses.
- **`src/quiz.js`** contains the `Quiz` class, shuffled questions, score and answer state, progress, feedback, explanations, results, and incorrect-answer review.
- **`src/colors.js`** provides ANSI escape-code styling utilities.
- **`data/questions.json`** contains categories and multiple-choice questions.

## Technical Concepts Demonstrated

- Native ES modules with `"type": "module"`.
- Node.js standard-library modules including `fs/promises`, `path`, `url`, and `readline`.
- Promises and asynchronous terminal interaction.
- JSON loading and parsing.
- Classes, getters, and state management.
- Fisher–Yates shuffling without mutating the original array.
- Input validation and replay control flow.
- ANSI escape sequences for terminal formatting.
- Node.js executable startup through the shebang in `index.js`.

## Testing

Run:

```bash
npm test
```

This executes Node.js's built-in test runner through `node --test`. No test files are present in the retrieved `v1` tree, so the repository does not currently provide automated test cases or coverage to document.

## Troubleshooting

- **Unsupported Node.js:** Install Node.js 18 or newer and check with `node --version`.
- **Questions cannot load:** Run from the project root and verify that `data/questions.json` exists and contains valid JSON.
- **ANSI escape codes appear:** Use a terminal that supports ANSI styling.
- **A question-count option is missing:** 3-question and 5-question options require at least that many questions in the selected category.

## Customization

- Add categories and questions in `data/questions.json`.
- Preserve each category's `name` and `questions` properties.
- Give each question a valid options array, zero-based answer index, and explanation.
- Adjust styles in `src/colors.js`.
- Change menu flow in `index.js`.
- Change scoring, feedback, progress, or result presentation in `src/quiz.js`.

## License

This project is licensed under the MIT License, as declared in `package.json`.
