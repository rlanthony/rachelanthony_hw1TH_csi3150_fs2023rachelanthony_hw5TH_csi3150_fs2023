# Creating a Quiz App using JavaScript, HTML, and CSS

## Problem Statement

Create an interactive quiz app that prompts users to answer a multiple-choice quiz question one at a time within a time limit. The correct answer and incorrect answer (if selected) will be identified visually as soon as the user selects an answer, or the time runs out. Once the user answers the last question, they will receive the score in addition to being given the opportunity to replay the quiz or exit it. This app will consist of a single HTML page that dynamically changes based on where the user is at in the quiz progression using JavaScript. Appropriate CSS styling will be appplied.

## Features of the App

- A home page that includes a start quiz button
- An instructions page outlining how to take the quiz and the rules of it that immediately follows the home page
  - Two buttons are given on the instructions page: (1) Exit the quiz (2) Start the quiz
- Each quiz question is show on its own page
  - The format of the questions is multiple choice
  - The user gets 15 seconds to answer the question before the correct answer is revealed
  - The timer is in the form of a countdown in addition to an dynamic progress bar that shows how much time as elaspsed
  - Once a user selects an answer, the timer and progress bar stops
    - If the answer is correct, it will highlight it in green with a checkmark icon
    - If the user selects an incorrect answer, the answer will be highlighted red with an X and the right answer will be highlighted green with a checkmark
  - The user's progress in the quiz is shown on each quiz page (e.g. Question X of X)
  - Once the timer runs out or the user selects an answer, a button to move to the next question appears
- Once all questions are completed, a quiz summary is provided that shows how many answers the user answered correctly
  - The user is given the ability to either replay the quiz and go back to question 1, or exit the quiz and return to the home page

## Directory Structure

- hw5-rachelAnthony_quizDemo /\* Root Directory \*/
  - css /\* Contains all CSS files \*/
    - style.css /\* Provides styling for the index.html page \*/
  - js /\* Contains all JS files \*/
    - questions.js /\* Contains an array of quiz questions objects with the question, correct answer, and multiple choice options for each \*/
    - quizApp.js /\* Contains all the code for the interactivity of the app \*/
  - .gitignore /\* Lists the files that are not posted to the GitHub repository \*/
  - index.html /\* Sets up the layout of the quiz app interface and imports the CSS and JS files \*/

## Creating the Quiz App

### Step 1: Creating your index.html file

1. In the head section of your index.html file, link your CSS and JS files to your HTML document. Also, provide a link to a font source (if applicable).

```html
<!-- CSS FILE -->
<link rel="stylesheet" href="css/style.css" />

<!-- Add questions list -->
<script src="js/questions.js" defer></script>

<!-- Main logic of the app -->
<script src="js/quizApp.js" defer></script>

<!-- This is my personal font awesome kit code. you will have to add your own after you register with email -->
<script
  src="https://kit.fontawesome.com/4a4f4b55b0.js"
  crossorigin="anonymous"
></script>
```

2. Now, create the elements for the body of the HTML document. Create a div that holds the start button for the quiz.

```html
<div class="start_btn"><button>Start Quiz</button></div>
```

3. Next, create a div that holds all of the instructions, and wrap each of them in a div. Also, create the two buttons that will allow users to either continue to the quiz or return to the home page.

```html
<div class="info_box">
  <div class="info-title"><span>Some Rules of this Quiz</span></div>
  <div class="info-list">
    <div class="info">
      1. You will have only <span>15 seconds</span> per each question.
    </div>
    <div class="info">2. Once you select your answer, it can't be undone.</div>
    <div class="info">3. You can't select any option once time goes off.</div>
    <div class="info">
      4. You can't exit from the Quiz while you're playing.
    </div>
    <div class="info">
      5. You'll get points on the basis of your correct answers.
    </div>
  </div>
  <div class="buttons">
    <button class="quit">Exit Quiz</button>
    <button class="restart">Continue</button>
  </div>
</div>
```

4. Create a div that will hold the quiz questions.

```html
<div class="quiz_box"></div>
```

5. Within the quiz_box div, make a header, section and footer.

   1. In the header, make the title, divs for the timer, and a div for the time line.

   ```html
   <header>
     <div class="title">Demo Quiz App in JavaScript</div>
     <div class="timer">
       <div class="time_left_txt">Time Left</div>
       <!-- The timer_sec div will be used in the quizApp.js file to overwrite to countdown -->
       <div class="timer_sec">15</div>
     </div>
     <!-- the time_line div provides the container that will hold the dynamic progress bar -->
     <div class="time_line"></div>
   </header>
   ```

   2. In the section, the quiz question and answers will be held. Create two divs, one for the question and one for the answer options.

   ```html
   <section>
     <div class="que_text">
       <!-- Insert questions from ./js/questions.js -->
     </div>
     <div class="option_list">
       <!-- Insert options to questions from ./js/questions.js -->
     </div>
   </section>
   ```

   3. In the footer, create a div that will hold the quiz progress and the button to move to the next question.

   ```html
   <footer>
     <div class="total_que">
       <!-- insert Question Count Number dynamically from JavaScript App logic -->
     </div>
     <button class="next_btn">Next Que</button>
   </footer>
   ```

   4. After inserting the header, section, and footer into the quiz_box div, ensure to close the div.

6. Finally, create the div that will hold the results of the quiz that is displayed once all questions are answered. Within the div, include text that indicates the quiz is over, create a div that will act as a placeholder for the number of questions answered correctly, and make a div with buttons to replay the quiz or exit it.

```html
    div class="result_box">
        <div class="icon">
            <i class="fas fa-crown"></i>
        </div>
        <div class="complete_text">You've completed the Quiz!</div>
        <div class="score_text">
            <!-- insert dynamic user score as Result from JavaScript -->
        </div>
        <div class="buttons">
            <button class="restart">Replay Quiz</button>
            <button class="quit">Quit Quiz</button>
        </div>
    </div>
```

This completes the HTML file.

### Step 2: Styling your index.html file using CSS in your style.css file

1. Import fonts to use throughout the entire interface.

```scss
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap");
```

2. Remove any default browser styling in terms of padding and margins. Also, set the box-sizing property to border-box so that the border and padding is taken into account with sizing. Finally, apply the fonts for the entire document.

```scss
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}
```

3. Provide a background color for the entire body, which will be the entire screen background.

```scss
body {
  background: #a020f0;
}
```

4. In the next code section, style all body content if a user highlights it. In this case, the text will turn white and have a purple background.

```scss
::selection {
  color: #fff;
  background: #a020f0;
}
```

5. To get the main elements (start button, instructions container, quiz question container, and results container) on each of the pages centered, use the following specifications for the position, top, left, and transform property. Also, add a shadow to each of the elements so that they stand out on the page.

```scss
.start_btn,
.info_box,
.quiz_box,
.result_box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
```

6. The following styling allows the active information to move on top of other elements to cover it up when the element is called.

   - The opacity at 1 makes the background completely opaque.
   - A z-index of 5 will position the element on the top of the stack because it is the greatest index out of all the elements.
   - Setting the pointer-events to auto makes the cursor act as if no pointer-event was specified and uses the default browser settings.
   - Setting the scale of the element to 1 will keep it the size that is specified in other areas of the CSS code and the transform centers the element on the page.

```scss
.info_box.activeInfo,
.quiz_box.activeQuiz,
.result_box.activeResult {
  opacity: 1;
  z-index: 5;
  pointer-events: auto;
  transform: translate(-50%, -50%) scale(1);
}
```

7. To style the start button seen on the home page of the game, the following changes were made to change the colors, text sizes, curvature of the element container, and change the cursor to a pointer when a user hovers over the button.

```scss
.start_btn button {
  font-size: 25px;
  font-weight: 500;
  color: #a020f0;
  padding: 15px 30px;
  outline: none;
  border: none;
  border-radius: 5px;
  background: #fff;
  cursor: pointer;
}
```

8. For the instructions, quiz, and results container, its size is set, in addition to the background color, curvature of the corners, and opacity. The opacity at 0 allows it to not be seen except for when it is the active box. A transition is also applied to smoothly move to the next page.

```scss
.info_box {
  width: 540px;
  background: #fff;
  border-radius: 5px;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.quiz_box {
  width: 550px;
  background: #fff;
  border-radius: 5px;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.result_box {
  background: #fff;
  border-radius: 5px;
  display: flex;
  padding: 25px 30px;
  width: 450px;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}
```

9. Next, set the styling for the content of the instructions box using the following settings.

For the title:

```scss
.info_box .info-title {
  height: 60px;
  width: 100%;
  border-bottom: 1px solid lightgrey;
  display: flex;
  align-items: center;
  padding: 0 30px;
  border-radius: 5px 5px 0 0;
  font-size: 20px;
  font-weight: 600;
}
```

For the ordered list:

```scss
.info_box .info-list {
  padding: 15px 30px;
}

.info_box .info-list .info {
  margin: 5px 0;
  font-size: 17px;
}
```

10. To specially format the time limit text, use this selector to reach that child element.

```scss
.info_box .info-list .info span {
  font-weight: 600;
  color: #a020f0;
}
```

11. To format the containers of the instructions buttons so that they are positioned to the right, a flex display coupled with a justification of flex-end. Additional styling is applied for visual clarity.

```scss
.info_box .buttons {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 30px;
  border-top: 1px solid lightgrey;
}
```

12. To style the actual buttons, an additional selector is added to identify it and the following styles are applied. The transition property will ease into the change color effect when the buttons are hovered over.

```scss
.info_box .buttons button {
  margin: 0 5px;
  height: 40px;
  width: 100px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  border: none;
  outline: none;
  border-radius: 5px;
  border: 1px solid #a020f0;
  transition: all 0.3s ease;
}
```

13. For the header of the quiz questions container, these various styles are applied to define the size and shape of the container. The display, aligh-items, and justify-content properties equally space and center the immediate child elements of the quiz_box div. The z-index will overlay the header over the container.

```scss
.quiz_box header {
  position: relative;
  z-index: 2;
  height: 70px;
  padding: 0 30px;
  background: #fff;
  border-radius: 5px 5px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0px 3px 5px 1px rgba(0, 0, 0, 0.1);
}
```

14. Now, in terms of the immediate children of the quiz-box class, these styling rules are applied to align and size them properly for the container.

```scss
.quiz_box header .title {
  font-size: 20px;
  font-weight: 600;
}

.quiz_box header .timer {
  color: #682a8f;
  background: #cce5ff;
  border: 1px solid #b8daff;
  height: 45px;
  padding: 0 8px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 145px;
}

.quiz_box header .timer .time_left_txt {
  font-weight: 400;
  font-size: 17px;
  user-select: none;
}

.quiz_box header .timer .timer_sec {
  font-size: 18px;
  font-weight: 500;
  height: 30px;
  width: 45px;
  color: #fff;
  border-radius: 5px;
  line-height: 30px;
  text-align: center;
  background: #343a40;
  border: 1px solid #343a40;
  user-select: none;
}

/* width of time line will be determined in the quizApp.js file */
.quiz_box header .time_line {
  position: absolute;
  bottom: 0px;
  left: 0px;
  height: 3px;
  background: #a020f0;
}
```

15. The section tag used to hold the quiz questions has the following background styling and padding. The question texts, and options are also fomatted to improve the design for clarity by specifying a size and spacing of the containers.

```scss
section {
  padding: 25px 30px 20px 30px;
  background: #fff;
}

section .que_text {
  font-size: 25px;
  font-weight: 600;
}

section .option_list {
  padding: 20px 0px;
  display: block;
}

section .option_list .option {
  background: aliceblue;
  border: 1px solid #84c5fe;
  border-radius: 5px;
  padding: 8px 15px;
  font-size: 17px;
  margin-bottom: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

section .option_list .option:last-child {
  margin-bottom: 0px;
}
```

Note: the transition for the .option pertains to the following effects applied to the different quiz question answer choices.

16. Style the different effects of the quiz question answer choices based on whether the user hovers over it, if the user answers correctly, or if the user answers incorrectly.

```scss
section .option_list .option:last-child {
  margin-bottom: 0px;
}

section .option_list .option:hover {
  color: #601391;
  background: #cce5ff;
  border: 1px solid #b8daff;
}

section .option_list .option.correct {
  color: #155724;
  background: #d4edda;
  border: 1px solid #c3e6cb;
}

section .option_list .option.incorrect {
  color: #721c24;
  background: #f8d7da;
  border: 1px solid #f5c6cb;
}

section .option_list .option.disabled {
  pointer-events: none;
}
```

17. As mentioned in the App Features, icons are also used to indicate the correct and incorrect answers. To style the different icons, these styling rules are applied to change the colors and sizes of them.

```scss
section .option_list .option .icon {
  height: 26px;
  width: 26px;
  border: 2px solid transparent;
  border-radius: 50%;
  text-align: center;
  font-size: 13px;
  pointer-events: none;
  transition: all 0.3s ease;
  line-height: 24px;
}
.option_list .option .icon.tick {
  color: #23903c;
  border-color: #23903c;
  background: #d4edda;
}

.option_list .option .icon.cross {
  color: #a42834;
  background: #f8d7da;
  border-color: #a42834;
}
```

18. The footer is going to have a similar layout as the header with centered and equally spaced items horizontally. There are also special font decorations added to bold the quiz question numbers as indicated by the p tags.

```scss
footer {
  height: 60px;
  padding: 0 30px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid lightgrey;
}

footer .total_que span {
  display: flex;
  user-select: none;
}

footer .total_que span p {
  font-weight: 500;
  padding: 0 5px;
}

footer .total_que span p:first-child {
  padding-left: 0px;
}
```

19. The footer has the button that allows the user to move to the next question, and its opacity will be set to 0 because it should not appear always, only after an answer has been selected or time runs out. The rest of the style properties are to make the button comply with the general styling of the quiz and be properly spaced and placed with the elements around it. It is also given an effect when its hovered over.

```scss
footer button {
  height: 40px;
  padding: 0 13px;
  font-size: 18px;
  font-weight: 400;
  cursor: pointer;
  border: none;
  outline: none;
  color: #fff;
  border-radius: 5px;
  background: #a020f0;
  border: 1px solid #a020f0;
  line-height: 10px;
  opacity: 0; /* hides the button */
  pointer-events: none;
  transform: scale(0.95);
  transition: all 0.3s ease;
}

footer button:hover {
  background: #7803c0;
}

footer button.show {
  opacity: 1; /* makes the button appear */
  pointer-events: auto;
  transform: scale(1);
}
```

20. For the results container, these colors, sizing, and spacing were applied so that the information is layed out vertically. The span tags and p tags are used to specially format the score outcome. The icon also is styled to fit the container.

```scss
.result_box .icon {
  font-size: 100px;
  color: #a020f0;
  margin-bottom: 10px;
}

.result_box .complete_text {
  font-size: 20px;
  font-weight: 500;
}

.result_box .score_text span {
  display: flex;
  margin: 10px 0;
  font-size: 18px;
  font-weight: 500;
}

.result_box .score_text span p {
  padding: 0 4px;
  font-weight: 600;
}
```

21. The container for all the buttons is given a flex display to space them and similar styling to the buttons on the other pages are applied in terms of the buttons itself and the pseudo-class used for when different events happen such as hover or if it is the restart or quit button. Changes in colors are used to alert the users when buttons are hovered over.

```scss
.result_box .buttons {
  display: flex;
  margin: 20px 0;
}

.result_box .buttons button {
  margin: 0 10px;
  height: 45px;
  padding: 0 20px;
  font-size: 18px;
  font-weight: 500;
  cursor: pointer;
  border: none;
  outline: none;
  border-radius: 5px;
  border: 1px solid #a020f0;
  transition: all 0.3s ease;
}

.buttons button.restart {
  color: #fff;
  background: #a020f0;
}

.buttons button.restart:hover {
  background: #8304d1;
}

.buttons button.quit {
  color: #a020f0;
  background: #fff;
}

.buttons button.quit:hover {
  color: #fff;
  background: #8604d6;
}
```

### Step 3: Creating an Array of Objects to Store the Question Data in the questions.js File

#### Part 1: Defining the Question Object

An array is used to hold the individual question objects to be used to loop through in the quizApp.js file.

Each object has the following structure:

- numb: Number
- question: String
- answer: String
- options: Array of Strings

Note: The `numb` field acts as the ID for the question and the `options` array stores four strings that are the answer choices the user can select from in the questions.

#### Part 2: Creation the `questions` Array

`let` is used to create the `questions` array so that it can be reassigned values in the quizApp.js file if questions are modified.

##### Step 1: Create the empty array

```javascript
let questions = [];
```

##### Step 2: Add question objects to the array

```javascript
let questions = [
  {
    numb: 1,
    question: "What does HTML stand for?",
    answer: "Hyper Text Markup Language",
    options: [
      "Hyper Text Preprocessor",
      "Hyper Text Markup Language",
      "Hyper Text Multiple Language",
      "Hyper Tool Multi Language",
    ],
  },
  {
    numb: 2,
    question: "What does CSS stand for?",
    answer: "Cascading Style Sheet",
    options: [
      "Common Style Sheet",
      "Colorful Style Sheet",
      "Computer Style Sheet",
      "Cascading Style Sheet",
    ],
  },
  {
    numb: 3,
    question: "What does PHP stand for?",
    answer: "Hypertext Preprocessor",
    options: [
      "Hypertext Preprocessor",
      "Hypertext Programming",
      "Hypertext Preprogramming",
      "Hometext Preprocessor",
    ],
  },
  {
    numb: 4,
    question: "What does SQL stand for?",
    answer: "Structured Query Language",
    options: [
      "Stylish Question Language",
      "Stylesheet Query Language",
      "Statement Question Language",
      "Structured Query Language",
    ],
  },
  {
    numb: 5,
    question: "What does XML stand for?",
    answer: "eXtensible Markup Language",
    options: [
      "eXtensible Markup Language",
      "eXecutable Multiple Language",
      "eXTra Multi-Program Language",
      "eXamine Multiple Language",
    ],
  },
  // Duplicate the object declaration and initialization above for more questions
];
```

### Step 4: Write the Logic in the quizApp.js File to Make the Quiz Interactive

#### Part 1: Use the `querySelector()` method to get the elements from the HTML document into the JS file and initialize quiz variables that control its operation

Note: `querySelector()` accepts parameters as identified in CSS and will return the first instance of the element.

```javascript
// get elememts from HTML document
const start_btn = document.querySelector(".start_btn button");
const info_box = document.querySelector(".info_box");
const exit_btn = info_box.querySelector(".buttons .quit");
const continue_btn = info_box.querySelector(".buttons .restart");
const quiz_box = document.querySelector(".quiz_box");
const result_box = document.querySelector(".result_box");
const restart_quiz = result_box.querySelector(".buttons .restart");
const quit_quiz = result_box.querySelector(".buttons .quit");
const option_list = document.querySelector(".option_list");
const time_line = document.querySelector("header .time_line");
const timeText = document.querySelector(".timer .time_left_txt");
const timeCount = document.querySelector(".timer .timer_sec");
const next_btn = document.querySelector("footer .next_btn");
const bottom_ques_counter = document.querySelector("footer .total_que");

//intialize quiz variables
let timeValue = 15;
let que_count = 0;
let que_numb = 1;
let userScore = 0;
let counter;
let counterLine;
let widthValue = 0;
```

#### Part 2: Add the event listeners for the buttons to know which box to show/hide

##### 1. Start Button

Once `start_btn` is clicked, the `activeInfo` class will be added to the `info_box` element, and because of our CSS file, `info_box` will appear because its opacity changes from 0 to 1 once assigned the `activeInfo` class.

```javascript
// if startQuiz button clicked
start_btn.addEventListener("click", (e) => {
  info_box.classList.add("activeInfo"); //show info box
});
```

##### 2. Exit Quiz Button (on instructions page)

The `exit_btn` on the instructions page will remove the `activeInfo` class from the `info_box` which changes its opacity back to 0 so that it disappears and thus, the `start_btn` will appear again.

```javascript
// if exitQuiz button clicked
exit_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
});
```

##### 3. Continue to Quiz Button (on instructions page)

When clicked, `activeInfo` class will be removed from the `info_box` to hide it by changing its opacity to 0 and `activeQuiz` class will be added to the `quiz_box` so that it appears.

Several methods will also be called to trigger events such as showing the first question, incrementing the question count, starting the timer, and starting the time line. These functions will be defined below.

```javascript
// if continueQuiz button clicked
continue_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.add("activeQuiz"); //show quiz box
  showQuetions(0); //calling showQestions function
  queCounter(1); //passing 1 parameter to queCounter
  startTimer(15); //calling startTimer function
  startTimerLine(0); //calling startTimerLine function
});
```

##### 4. Restart Quiz Button (on results page)

This button will move to the start of the quiz by redefining the quiz variables, showing `quiz_box`, and hiding `results_box`. It will also clear out the timer and restart it.

```javascript
// if restartQuiz button clicked
restart_quiz.addEventListener("click", (e) => {
  quiz_box.classList.add("activeQuiz"); //show quiz box
  result_box.classList.remove("activeResult"); //hide result box
  timeValue = 15;
  que_count = 0;
  que_numb = 1;
  userScore = 0;
  widthValue = 0;
  showQuetions(que_count); //calling showQestions function
  queCounter(que_numb); //passing que_numb value to queCounter
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  startTimer(timeValue); //calling startTimer function
  startTimerLine(widthValue); //calling startTimerLine function
  timeText.textContent = "Time Left"; //change the text of timeText to Time Left
  next_btn.classList.remove("show"); //hide the next button
});
```

##### 4. Quit Quiz Button (on results page)

When the `quit_quiz` button is clicked, the current window is reloaded that takes the user back to the home page with the start quiz button and converts all variables back to their default values.

```javascript
// if quitQuiz button clicked
quit_quiz.addEventListener("click", (e) => {
  window.location.reload(); //reload the current window
});
```

##### 5. Next Question Button (on each question page)

First, you need to ensure that you are not at the last question in the `questions` array or else you will get an error. You can do this by using an if statement to see if the question count is less than the length of the array minus 1 since question count was orignally set to 0.

```javascript
next_btn.addEventListener("click", (e) => {
  if (que_count < questions.length - 1) {
  } else {
  }
});
```

If it is not the last question, `que_count` and `que_numb` are incremented by one to pass into the `showQuestions()` and `queCounter()` functions to move to the next question.

```javascript
next_btn.addEventListener("click", (e) => {
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
  } else {
  }
});
```

The `counter` and `counterLine` will be cleared so that the timer restarts once you move to the next question. The `timeText` content is changed back to "Time Left" in case the user ran out of time and it changed to "Time Off".

```javascript
next_btn.addEventListener("click", (e) => {
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    startTimer(timeValue); //calling startTimer function
    startTimerLine(widthValue); //calling startTimerLine function
    timeText.textContent = "Time Left"; //change the timeText to Time Left
  } else {
  }
});
```

The `next_btn` is now hidden until the user selects an answer or the time runs out.

```javascript
next_btn.addEventListener("click", (e) => {
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    startTimer(timeValue); //calling startTimer function
    startTimerLine(widthValue); //calling startTimerLine function
    timeText.textContent = "Time Left"; //change the timeText to Time Left
    next_btn.classList.remove("show"); //hide the next button
  } else {
  }
});
```

If it is the last question in the `questions` array, the two counters are cleared and the `results_box` is shown by calling the `showResult()` function.

```javascript
// if Next Question button is clicked
next_btn.addEventListener("click", (e) => {
  //check if it does not exceed max questions
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    startTimer(timeValue); //calling startTimer function
    startTimerLine(widthValue); //calling startTimerLine function
    timeText.textContent = "Time Left"; //change the timeText to Time Left
    next_btn.classList.remove("show"); //hide the next button
  } else {
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    showResult(); //calling showResult function
  }
});
```

This is the complete code for the `next_btn` on click event listener.

#### Part 3: Write functions to call in the button event listeners

##### 1. Function to show the questions

This function will grab the `que_text` placeholder created in the HTML DOM and change its `innerHTML` property based on the question.

The specific question is accessed by passing the index of the element to be accessed in the array which is the `que_count` used in the button event listeners.

\<span> tags are used to ensure the text is written on one line.

```javascript
function showQuetions(index) {
  const que_text = document.querySelector(".que_text");

  //creating a new span and div tag for question and option and passing the value using array index
  let que_tag =
    "<span>" +
    questions[index].numb +
    ". " +
    questions[index].question +
    "</span>";

  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
}
```

Next, the different multiple choice options are populated in a similar manner by passing the index of the element to be accessed in the array but also needs to access the inner `options` array. We will manually account for the 4 choices, but this code needs to be modified if the number of answer choices deviates from 4. This new variable is set to the `options_list` innerHTML.

```javascript
function showQuetions(index) {
  //...above code

  let option_tag =
    '<div class="option"><span>' +
    questions[index].options[0] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[1] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[2] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[3] +
    "</span></div>";
  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
  option_list.innerHTML = option_tag; //adding new (child) div tag inside option_tag
}
```

Lastly, to make all of the options clickable, the `onclick` attribute will be set for each of the options using a for loop.

```javascript
function showQuetions(index) {
  //...above code

  const option = option_list.querySelectorAll(".option");

  // set onclick attribute to all available options
  for (i = 0; i < option.length; i++) {
    option[i].setAttribute("onclick", "optionSelected(this)");
  }
}
```

Putting all the code together for the showQuetion() function:

```javascript
function showQuetions(index) {
  const que_text = document.querySelector(".que_text");

  //creating a new span and div tag for question and option and passing the value using array index
  // self exercise: re-write the following using backtick syntax for string in JS instead of concatenation operator
  let que_tag =
    "<span>" +
    questions[index].numb +
    ". " +
    questions[index].question +
    "</span>";
  let option_tag =
    '<div class="option"><span>' +
    questions[index].options[0] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[1] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[2] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[3] +
    "</span></div>";
  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
  option_list.innerHTML = option_tag; //adding new (child) div tag inside option_tag

  const option = option_list.querySelectorAll(".option");

  // set onclick attribute to all available options
  for (i = 0; i < option.length; i++) {
    option[i].setAttribute("onclick", "optionSelected(this)");
  }
}
```

##### 2. Function to call when an option is selected

As stated in the app requirements, icons are used in addition to colors to identify the correct and incorrect answers. New div tags are created to embed them in the HTML DOM.

```javascript
// create new div tags for right or wrong tick icons
let tickIconTag = '<div class="icon tick"><i class="fas fa-check"></i></div>';
let crossIconTag = '<div class="icon cross"><i class="fas fa-times"></i></div>';
```

As soon as a user selects an option, the counters are stopped and cleared. We will also grab the user's answer, the correct answer to the question, and all of the options provided for the questions.
javascript

```
function optionSelected(answer) {
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  let userAns = answer.textContent; //getting user selected option
  let correcAns = questions[que_count].answer; //getting correct answer from array
  const allOptions = option_list.children.length; //getting all option items
}
```

If the user correctly answers the question, their score increases, and the answer is decorated with a green color by adding the `correct` class and the correct icon.

```javascript
function optionSelected(answer) {
  //..code above

  if (userAns == correcAns) {
    userScore += 1; //update total score value increment by 1
    answer.classList.add("correct"); //add green color to correct selected option
    answer.insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to correct selected option
    console.log("Correct Answer");
    console.log("Your correct answers = " + userScore);
  } else {
  }
}
```

If the user gets the question wrong, the appropriate styling is applied to the option selected by adding the `incorrect` class to the answer and the wrong answer icon. The correct answer is given the appropriate styling too by looping through all of the question answer options until it matches the answer and then applying the styling.

```javascript
function optionSelected(answer) {
  if (userAns == correcAns) {
    //..code above
  } else {
    answer.classList.add("incorrect"); //add red color to correct selected option
    answer.insertAdjacentHTML("beforeend", crossIconTag); //add cross icon to correct selected option
    console.log("Wrong Answer");

    for (i = 0; i < allOptions; i++) {
      if (option_list.children[i].textContent == correcAns) {
        //if there is an option which is matched to an array answer
        option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
        option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
        console.log("Auto selected correct answer.");
      }
    }
  }
}
```

Also, once an option is selected, disable all the buttons so that they cannot select another one and display the `next_btn` by using the `show` class.

```javascript
function optionSelected(answer) {
    /..code above

 for (i = 0; i < allOptions; i++) {
    option_list.children[i].classList.add("disabled"); //once user select an option, disable all options
  }
  next_btn.classList.add("show"); //show the next button if user selected any option
};
```

The complete code the `optionSelected()` is as follows:

```javascript
//if user clicked on option
function optionSelected(answer) {
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  let userAns = answer.textContent; //getting user selected option
  let correcAns = questions[que_count].answer; //getting correct answer from array
  const allOptions = option_list.children.length; //getting all option items

  //if user selected option is equal to array's correct answer
  if (userAns == correcAns) {
    userScore += 1; //update total score value increment by 1
    answer.classList.add("correct"); //add green color to correct selected option
    answer.insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to correct selected option
    console.log("Correct Answer");
    console.log("Your correct answers = " + userScore);
  } else {
    answer.classList.add("incorrect"); //add red color to correct selected option
    answer.insertAdjacentHTML("beforeend", crossIconTag); //add cross icon to correct selected option
    console.log("Wrong Answer");

    for (i = 0; i < allOptions; i++) {
      if (option_list.children[i].textContent == correcAns) {
        //if there is an option which is matched to an array answer
        option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
        option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
        console.log("Auto selected correct answer.");
      }
    }
  }
  for (i = 0; i < allOptions; i++) {
    option_list.children[i].classList.add("disabled"); //once user select an option, disable all options
  }
  next_btn.classList.add("show"); //show the next button if user selected any option
}
```

##### 3. Function to call to display the results box

First, we need to remove the `info_box` and `quiz_box` since the elements are being placed on top of the other. After this, we can apply the `activeResult` class to show the `result_box`.

```javascript
function showResult() {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.remove("activeQuiz"); //hide quiz box
  result_box.classList.add("activeResult"); //show result box
}
```

Second, we will get the \<div> created in the HTML DOM for the score text so that we can change the innerHTML property to show a message with the score. The message displayed will change based on if the user got more than 3, more than 1, or 0 correct. The `userScore` holds the number of questions users answered correctly and was updated each time the `optionSelected()` method was called.

```javascript
// display for result box based on user performance
function showResult() {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.remove("activeQuiz"); //hide quiz box
  result_box.classList.add("activeResult"); //show result box
  const scoreText = result_box.querySelector(".score_text");
  if (userScore > 3) {
    // if user scored more than 3
    //create a new span tag and pass the user score number and total question number
    let scoreTag =
      "<span>and congrats! , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag; //add new span tag inside score_Text
  } else if (userScore > 1) {
    // if user scored more than 1
    let scoreTag =
      "<span>and nice , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  } else {
    // if user scored less than 1
    let scoreTag =
      "<span>and sorry , You got only <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  }
}
```

##### 4. Function to start the timer

Note: `setInterval` and `clearInterval` are global functions used in JavaScript to handle time.

We set our counter variable to the output of the `setInterval` function that takes the timer the function we will define below and 1000 milliseconds.

```javascript
// control the timer and actions associated to it
function startTimer(time) {
  counter = setInterval(timer, 1000);
}
```

The `timeCount` variable will display what ever the time is and then `time` is decremented by 1.

If our time value is a single digit, we will add a zero in front of it in the `timeCount` variable.

```javascript
// control the timer and actions associated to it
function startTimer(time) {
  //...above code

  function timer() {
    timeCount.textContent = time; //change the value of timeCount with time value
    time--; //decrement the time value
    if (time < 9) {
      //if timer is less than 9
      let addZero = timeCount.textContent;
      timeCount.textContent = "0" + addZero; //add a 0 before time value
    }
  }
}
```

If our time has run out (time < 0>), the counter is cleared out, the text that shows the time changes from "Time Left" to "Time Off" and the correct answer is shown using similar code as in the `optionSelected()` function. We will also disable the ability to click all of the option buttons.

```javascript
function timer() {
  //... above code

  if (time < 0) {
    //if timer is less than 0
    clearInterval(counter); //clear counter
    timeText.textContent = "Time Off"; //change the time text to time off
    const allOptions = option_list.children.length; //get all option items
    let correcAns = questions[que_count].answer; //get correct answer from array
    for (i = 0; i < allOptions; i++) {
      if (option_list.children[i].textContent == correcAns) {
        //if there is an option which is matched to an array answer
        option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
        option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
        console.log("Time Off: Auto selected correct answer.");
      }
    }
    for (i = 0; i < allOptions; i++) {
      option_list.children[i].classList.add("disabled"); //once user select an option then disabled all options
    }
    next_btn.classList.add("show"); //show the next button if user selected any option
  }
}
```

Complete code for the `startTimer()` function:

```javascript
// control the timer and actions associated to it
function startTimer(time) {
  counter = setInterval(timer, 1000);
  function timer() {
    timeCount.textContent = time; //change the value of timeCount with time value
    time--; //decrement the time value
    if (time < 9) {
      //if timer is less than 9
      let addZero = timeCount.textContent;
      timeCount.textContent = "0" + addZero; //add a 0 before time value
    }
    if (time < 0) {
      //if timer is less than 0
      clearInterval(counter); //clear counter
      timeText.textContent = "Time Off"; //change the time text to time off
      const allOptions = option_list.children.length; //get all option items
      let correcAns = questions[que_count].answer; //get correct answer from array
      for (i = 0; i < allOptions; i++) {
        if (option_list.children[i].textContent == correcAns) {
          //if there is an option which is matched to an array answer
          option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
          option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
          console.log("Time Off: Auto selected correct answer.");
        }
      }
      for (i = 0; i < allOptions; i++) {
        option_list.children[i].classList.add("disabled"); //once user select an option then disabled all options
      }
      next_btn.classList.add("show"); //show the next button if user selected any option
    }
  }
}
```

##### 5. Function to start the timer line

First, the `counterLine` is initialized using the `setInterval` method again as takes the timer and 29 milliseconds as an argument

Second, a `timer()` function is created that increments the time by 1 and proportionaly sets the width of the `counterLine` by the `time` value in pixels to show the growth. However, if the time has increased more than the size of the `quiz_box` container as indicated in the CSS file (set to width: 550), clear the `coutnerLine` so that it does not expand beyond the size of the \<div>.

```javascript
// Shows a progress bar mirroring timer value left
function startTimerLine(time) {
  counterLine = setInterval(timer, 29);
  function timer() {
    time += 1; //upgrading time value with 1
    time_line.style.width = time + "px"; //increasing width of time_line with px by time value
    if (time > 549) {
      //if time value is greater than 549
      clearInterval(counterLine); //clear counterLine
    }
  }
}
```

##### 6. Function to get the question's position in relation to the total amount of questions

We wanted to show something along the lines of "Question X of Y" on each of the `quiz_box` divs. To do this dynamically, we will pass a value, in this case of `que_numb` variable that we initialized to 1 in the beginning, and use a \<span> tag to format the string into the format "X of Y Questions."

```javascript
function queCounter(index) {
  //creating a new span tag and passing the question number and total question
  let totalQueCounTag =
    "<span><p>" +
    index +
    "</p> of <p>" +
    questions.length +
    "</p> Questions</span>";
  bottom_ques_counter.innerHTML = totalQueCounTag; //adding new span tag inside bottom_ques_counter
}
```

### This completes the guide on how to build a dynamic quiz app using HTML, CSS, and JS.
