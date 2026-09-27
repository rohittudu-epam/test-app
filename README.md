# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts. The application runs entirely on Node.js built-ins, presents a menu-driven terminal experience, randomizes questions, evaluates answers immediately, and provides a score summary with review guidance.

## Features

- Interactive category selection:
  - JavaScript Basics
  - Node.js Fundamentals
  - General Programming
- Choose to answer all available questions, three questions, or five questions when the category contains enough questions.
- Fisher–Yates shuffling of the selected questions for each quiz.
- Numbered answer selection with validation and retry prompts.
- Immediate correct/incorrect feedback and explanations from the question data.
- Visual progress bar and question counter.
- Final score and percentage with performance messages.
- Review of incorrect answers, including the selected and correct options.
- Option to start another quiz without restarting the process.
- ANSI terminal styling implemented without external runtime dependencies.

## Requirements

- Node.js 18.0.0 or newer. The required version is declared in `package.json`.
- A terminal capable of displaying standard ANSI escape sequences for the intended colors and symbols.

No npm runtime dependencies are declared; the application uses Node.js built-in modules only.

## Installation

Clone or download the repository, then change into the project directory:

```bash
git clone <repository-url>
cd test-app
```

There are no package dependencies to install. If you want npm to process the project manifest, run:

```bash
npm install
```

## Running the quiz

Start the application with:

```bash
npm start
```

This runs `node index.js`. You can also invoke the entry point directly:

```bash
node index.js
```

The application then guides you through this flow:

1. Select a category by entering its number.
2. Select the number of questions.
3. Press Enter to begin.
4. Select an answer for each question by entering its number.
5. Review feedback and explanations.
6. View the final results and optionally play again.

Invalid menu or answer input is rejected until a valid option number is entered. At the replay prompt, answers beginning with `y` are treated as confirmation; all other responses end the session.

## Testing

The package defines the following test command:

```bash
npm test
```

It runs Node.js's built-in test runner with `node --test`. No test files are currently included in the repository, so the command may complete without executing tests.

## Question data

Questions are stored in `data/questions.json`. The top-level `categories` object maps category IDs to category records. Each category contains a display `name` and a `questions` array. Each question uses this shape:

```json
{
  "question": "Question text",
  "options": ["First option", "Second option"],
  "answer": 0,
  "explanation": "Optional explanation"
}
```

`answer` is a zero-based index into `options`. The application loads this file relative to the entry-point location, so it does not depend on the current working directory.

To add or change quiz content, edit the JSON while preserving this structure. The category and question counts shown by the menus are generated from the file at startup.

## Architecture and project structure

```text
.
├── data/
│   └── questions.json   # Categories, questions, answer indexes, and explanations
├── src/
│   ├── colors.js        # ANSI color and text-style helpers
│   ├── input.js         # Readline interface and interactive prompt helpers
│   └── quiz.js          # Quiz state, shuffling, scoring, feedback, and results
├── index.js             # Application entry point and main interaction loop
├── package.json         # Project metadata, scripts, and Node.js requirement
└── README.md            # Project documentation
```

### Runtime flow

- `index.js` loads `data/questions.json` using `node:fs/promises`, builds the category and question-count menus, and controls the replay loop.
- `src/input.js` wraps Node's `readline` API in Promise-based helpers for selections, confirmations, and pause prompts.
- `src/quiz.js` creates a `Quiz` instance, shuffles questions, tracks progress and answers, and renders feedback and final results.
- `src/colors.js` provides ANSI escape-code formatting functions used by the interface.

## Configuration and environment

The repository does not define environment variables, configuration files, databases, network services, authentication, or external APIs. Quiz content is local JSON data and is read at runtime from the repository's `data` directory.

## License

The project declares an MIT license in `package.json`.

## Known repository notes

A `.DS_Store` file is present in the repository. It is macOS Finder metadata and is not used by the application; it is intentionally not documented as part of the runtime structure.
