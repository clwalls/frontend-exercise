<template>
  <div className="quiz">
    <h1>Quiz App</h1>
    <h3>Hello, welcome to the Cognito Forms quiz!</h3>
    <p>
      To get started, click the 'Start quiz' button. You can't go back once
      you've answered a question.
    </p>
    <br />
    <button v-if="!quizStarted" @click="start()">Start quiz</button>
    <div v-if="quizStarted">
      <div className="question">
        {{ this.prompts[questionNumber] }}
      </div>
      <div
        className="answerChoices"
        v-for="answers in answerChoices[questionNumber]"
      >
        <label>
          <input
            type="radio"
            name="currentQuestion"
            :value="answers"
            v-model="choice"
          />{{ answers }}</label
        >
      </div>
      <button v-if="!quizOver" @click="onClick()">
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
    onClick() {
      if (this.choice === null) return;
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
      this.quizStarted = false;
    },
  },
};
/* cognito blue: #00B3AB
orange: #D85427
gray: #D3DCE4
header darker blue: #234652
lime green: #AED136
*/
</script>
<style scoped></style>
