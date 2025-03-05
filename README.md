# Quiz app with react
## 📖 Overview
This is a React-based Quiz App that allows users to answer multiple-choice questions with a timer. The app tracks user responses and displays a summary at the end. It dynamically updates based on user input and provides immediate feedback on correct and incorrect answers. 
## 🌳Component tree
App
├──Header
├──main
|  ├── Quiz
|  |   ├──Question
|  |   |  ├──QuestionTimer
|  |   |  └──Answers
|  |   └──Summary

##  Component Breakdown:

## 🏗 Component Breakdown

### **App.jsx**
- **Purpose**: Serves as the root component that renders the main application structure.
- **Contains**:
  - `<Header />` - Displays the title or branding of the quiz.
  - `<main>` - Wraps the core quiz functionality.
  - `<Quiz />` - Manages the quiz state and logic.

### **Quiz.jsx**
- **Purpose**: Controls the quiz flow and user interactions.
- **State**:
  - `userAnswers` (array): Stores the user's selected answers.
  - `activeQuestionIndex` (derived state): Tracks the current question index.
  - `quizIsComplete` (boolean): Determines if the quiz is finished.
- **Contains**:
  - `<Question />` - Displays the current question.
  - `<Summary />` - Shown when the quiz is complete.
- **Event Handlers**:
  - `handleSelectAnswer(selectedAnswer)`: Adds an answer to `userAnswers` and moves to the next question.
  - `handleSkipAnswer()`: Calls `handleSelectAnswer(null)` to mark a question as skipped.

### **Question.jsx**
- **Purpose**: Displays an individual quiz question and manages answer selection.
- **State**:
  - `answer` (object): Tracks the selected answer and its correctness.
- **Contains**:
  - `<QuestionTimer />` - Manages countdown and auto-skip.
  - `<Answers />` - Displays answer choices.
- **Event Handlers**:
  - `handleSelectAnswer(answer)`: Updates the selected answer and determines if it's correct.

### **QuestionTimer.jsx**
- **Purpose**: Handles countdown functionality for each question.
- **State**:
  - `remainingTime` (number): Tracks the remaining time for the question.
- **Effects**:
  - Starts a timeout to trigger `onTimeout` when time runs out.
  - Updates the progress bar every 100ms.

### **Answers.jsx**
- **Purpose**: Displays multiple answer options and manages user selection.
- **Contains**:
  - `shuffledAnswers` (useRef): Randomizes answer order without re-rendering.
- **Event Handlers**:
  - `onSelect(answer)`: Calls `handleSelectAnswer` when an answer is clicked.

### **Summary.jsx**
- **Purpose**: Displays a summary of user performance after quiz completion.
- **Calculations**:
  - `skippedAnswersShare`: Percentage of skipped questions.
  - `correctAnswersShare`: Percentage of correct answers.
  - `wrongAnswersShare`: Percentage of incorrect answers.
- **Contains**:
  - Statistics on user performance.
  - List of questions with user answers and correctness status.

## 🔄 Hooks Used and Why

### `useState`
- **Used in**: `Quiz.jsx`, `Question.jsx`, `QuestionTimer.jsx`
- **Why?**
  - **`Quiz.jsx`**: Stores `userAnswers` to track the quiz progress.
  - **`Question.jsx`**: Stores the selected answer and correctness state.
  - **`QuestionTimer.jsx`**: Tracks the remaining time for each question.

### `useCallback`
- **Used in**: `Quiz.jsx`
- **Why?**
  - Optimizes `handleSelectAnswer` and `handleSkipAnswer` so they do not get recreated unnecessarily on re-renders.

### `useRef`
- **Used in**: `Answers.jsx`
- **Why?**
  - Stores the shuffled answers to maintain the same order throughout rendering.

### `useEffect`
- **Used in**: `QuestionTimer.jsx`
- **Why?**
  - Starts a timeout for `onTimeout` when the timer begins.
  - Updates the remaining time at a 100ms interval.

## 🛠 Customizing the Questions

We have a `questions.js` file that contains all quiz questions. Each question consists of a `text` (the question itself) and an `answers` array. **The first answer in the array is always the correct one**, so when modifying the questions, make sure the correct answer remains the first item.

### Example Structure:
```js
// The first item of each answers array is the correct one
// the answers are shuffled in app so the first item is not the correct one in app.
{
    id: 'q1',
    text: 'Which of the following definitions best describes React.js?',
    answers: [
    'A library to build user interfaces with help of declarative code.',
    'A library for managing state in web applications.',
    'A framework to build user interfaces with help of imperative code.',
    'A library used for building mobile applications only.',
    ],
}
```