<template>
  <div className="wrapper">
    <h1>Quizio</h1>
    <h3>Hello, welcome to the Cognito Forms quiz!</h3>
    <p>
      To get started, click the 'Start quiz' button. You can't go back once
      you've answered a question.
    </p>
    <button v-if="!quizStarted" @click="start()">
      Start quiz
    </button>
    <div v-if="quizStarted">
      <!-- v-if to make sure question at that index is not null/undefined-->
      <div className="question" v-if="this.prompts[questionNumber]">
        {{ `${questionNumber + 1}. ${this.prompts[questionNumber]}` }}
      </div>
      <div
        className="answerChoices"
        v-for="answers in answerChoices[questionNumber]"
      >
        <label class="container">
          <input
            class="radio__input"
            type="radio"
            name="currentQuestion"
            :value="answers"
            v-model="choice">
          </input> <span class="checkmark"></span>
          {{ answers }}
        </label>
      </div>
      <button
        v-if="choice == null && quizOver == false"
        class="btn-unclickable"
      >
        Submit Answer
      </button>
      <button
        v-if="choice != null && quizOver == false"
        class="btn-clickable"
        @click="submitAnswer"
      >
        Submit Answer
      </button>
    </div>
    <div v-if="quizOver" className="results">
      <h3>Your answers:</h3>
      <div v-for="(prompt, index) in prompts">
        {{ prompt }} {{ answers[index] }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // inside main.js we pass questions to this Quiz component stored as:
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      choice: null,
      prompts: [],
      answerChoices: [],
      answers: [],
      quizStarted: false,
      quizOver: false,
      questionNumber: 0,
    };
  },
  created() {
    this.setQuestionsAndAnswers();
  },
  methods: {
    async setQuestionsAndAnswers() {
      this.prompts = this.questions.map((prompt) => prompt.text);
      this.answerChoices = this.questions.map((choices) => choices.answers);
    },
    start() {
      this.quizStarted = true;
      this.questionNumber = 0;
    },
    submitAnswer() {
      if (this.choice == null) return;
      this.answers.push(this.choice);
      this.choice = null;
      this.questionNumber++;
      if (this.prompts.length == this.answers.length) this.reset();
    },
    // Displays all questions and their respective answers, and the user's answers.
    /* The questions provided are subjective and not all of them have a correct 
    answer (nor is one provided), so we don't calculate a score based on accuracy*/
    reset() {
      this.quizOver = true;
      this.quizStarted = true;
    },
  },
};
/* cognito blue: #00B3AB
orange: #D85427 #b32a00
gray: #D3DCE4
header darker blue: #D3DCE4
lime green: #AED136
*/
</script>
<style scoped>
button {
  max-width: 40%;
  font-size: 24px;
  border-radius: 14px;
  background-color: #aed136;
  color: white;
  border: 2px solid #234652;
  font-family: "Newsreader", serif;
  padding: 10px 10px 10px 10px;

  opacity: 0.6;
  cursor: pointer;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
button:hover {
  opacity: 1;
}
.btn-unclickable {
  cursor: not-allowed;
}
.btn-unclickable:hover {
  opacity: 0.6;
}
.btn-clickable {
  cursor: pointer;
}
.btn-clickable:hover {
  opacity: 1;
}
h3 {
  font-size: 28px;
  color: #234652;
}
h1 {
  color: #00b3ab;
  font-family: "Lobster", cursive;
  font-size: 60px;
}
label {
  cursor: pointer;
}
.answerChoices {
  position: relative;
}
/* Customize the label (the container) */
.container {

  display: block;
  position: relative;
  padding-left: -25px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom radio button */
.checkmark {
  position: absolute;
  top: 0;
  left: 450px;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the radio button is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.container .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}
</style>

<style>
*,
*:after,
*:before {
  box-sizing: border-box;
}
body {
  margin-right: 8%;
  margin-left: 8%;
  text-align: center;
  font-family: "Newsreader", serif;
  font-size: 24px;
  color: #234652;
}
</style>
