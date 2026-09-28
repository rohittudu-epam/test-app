# Quiz CLI

An interactive command-line quiz game for learning and reviewing JavaScript, Node.js, and general programming concepts.

## Overview

Quiz CLI is a dependency-free Node.js command-line application. It loads multiple-choice questions from `data/questions.json`, lets the player choose a category and quiz length, provides immediate answer feedback, and displays a final score with a review of incorrect answers.

The project uses native Node.js ES modules and requires Node.js 18 or newer.

## Features

- Interactive terminal gameplay using numbered menus and prompts.
- Three question categories:
  - JavaScript Basics
  - Node.js Fundamentals
  - General Programming
- Quiz lengths of all available questions, 3 questions, or 5 questions when the category contains enough questions.
- Fisher–Yates shuffling so questions are presented in a randomized order.
- Progress display and a visual progress bar during a quiz.
- Immediate correct/incorrect feedback and optional question explanations.
- Final percentage, performance message, and incorrect-answer review.
- Option to play another round without restarting the program.
- ANSI terminal color utilities implemented locally without external dependencies.

## Prerequisites

- Node.js `>=18.0.0`
- npm (included with Node.js)

## Installation

Clone or download the repository, then run the application from the project root:

```bash
npm install
```

The project declares no runtime or development dependencies, so installation does not download any third-party packages. Running `npm install` is still safe and establishes the normal npm project workflow.

## Running the Quiz

Start the application with:

```bash
npm start
```

The equivalent direct command is:

```bash
node index.js
```

## Gameplay Flow

1. The application loads and parses `data/questions.json`.
2. Choose a category from the numbered list.
3. Choose `All questions`, `3 questions`, or `5 questions` when that option is available.
4. Press Enter to begin.
5. For each question, select an answer from the numbered choices.
6. Review immediate feedback and the explanation, when provided.
7. Continue until all selected questions have been answered.
8. View the final score and incorrect-answer review.
9. Choose whether to play again, or exit with a closing message.

Questions are selected from the beginning of the category's question array after the category's questions have been shuffled. The selected quiz therefore contains a randomized subset of the category when fewer than all questions are requested.

## Question Data

Questions are stored in `data/questions.json`. The top-level object contains a `categories` object. Each category has a display `name` and a `questions` array. Each question contains multiple-choice text, an answer index, and an explanation. Conceptually, the structure is:

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

The retrieved data includes three categories with five questions each, for a total of 15 questions. The `answer` value is a zero-based index into the corresponding `options` array. When adding or editing questions, keep that index aligned with the correct option and preserve valid JSON syntax.

## Project Structure

```text
.
├── data/
│   └── questions.json   # Categories and multiple-choice question data
├── src/
│   ├── colors.js        # ANSI color and text-style helpers
│   ├── input.js          # Readline interface and async terminal prompts
│   └── quiz.js           # Quiz state, questions, scoring, feedback, and results
├── index.js              # Application entry point and gameplay orchestration
├── package.json          # npm metadata, scripts, and Node.js requirement
└── README.md             # Project documentation
```

`.DS_Store`, if present, is an operating-system metadata file and is not part of the application logic.

## Architecture and Module Responsibilities

### `index.js`

The executable entry point. It resolves the path to `data/questions.json`, loads and parses the question data, renders the category and question-count menus, manages the replay loop, and coordinates the `Quiz` and input modules. It also handles fatal errors and closes the readline interface in a `finally` block.

### `src/input.js`

Provides the terminal interaction layer. It creates a Node.js `readline` interface and exposes promise-based helpers for free-form prompts, numbered selection, yes/no confirmation, and waiting for Enter. Numeric selections are validated before being returned.

### `src/quiz.js`

Contains the exported `Quiz` class. It maintains the shuffled questions, current position, score, recorded answers, and category name. It exposes state getters, asks questions, records answers, renders progress, gives feedback, calculates results, and reviews incorrect answers.

### `src/colors.js`

Contains local ANSI escape-code utilities. It exports `colorize`, individual color/style helpers such as `green`, `cyan`, and `bold`, plus combined helpers for success, error, warning, info, and highlighted text.

## Technical Concepts Demonstrated

- Native ES modules with `"type": "module"`.
- Node.js standard-library modules, including `fs/promises`, `path`, `url`, and `readline`.
- Promise-based asynchronous terminal interaction.
- JSON file loading and parsing.
- Class-based state management with getters and a private shuffle method.
- Fisher–Yates array shuffling without mutating the original question list.
- Input validation and replay-loop control flow.
- ANSI escape sequences for terminal formatting.
- Command-line executable startup through the Node.js shebang in `index.js`.

## Testing

The package defines this test command:

```bash
npm test
```

It runs Node.js's built-in test runner via `node --test`. No test files are present in the retrieved `v1` repository tree, so the repository does not currently provide automated test cases or test coverage to document.

## Troubleshooting

### `node: command not found` or an unsupported Node.js version

Install Node.js 18 or newer and confirm the version:

```bash
node --version
```

### The application cannot load questions

Run the command from the repository project, and verify that `data/questions.json` exists and contains valid JSON. The application resolves this file relative to `index.js`.

### Terminal colors appear as escape codes

The application writes ANSI color sequences directly to the terminal. Use a terminal that supports ANSI styling, or run it in a terminal configuration that interprets ANSI escape codes.

### A question-count option is missing

The 3-question and 5-question options are shown only when the selected category contains at least that many questions. `All questions` is always available.

## Customization

- Add or edit categories and questions in `data/questions.json`.
- Keep each category's `name` and `questions` properties intact so the existing menu and quiz loader can use them.
- Ensure each question has a valid options array, a zero-based answer index, and an explanation.
- Adjust terminal colors or styles in `src/colors.js`.
- Change menu flow, startup behavior, or question-count rules in `index.js`.
- Change scoring, feedback, progress, or result presentation in `src/quiz.js`.

## License

This project is licensed under the MIT License, as declared in `package.json`.
