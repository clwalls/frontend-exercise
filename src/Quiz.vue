<template>
  <div className="container">
    <h1>Quizio</h1>
    <h3>Hello, welcome to the Cognito Forms quiz!</h3>
    <p>
      To get started, click the 'Start quiz' button. You can't go back once
      you've answered a question.
    </p>
    <button v-if="!quizStarted" @click="start()">Start quiz</button>
    <div v-if="quizStarted">
      <!-- v-if to make sure question at that index is not null/undefined-->
      <div className="question" v-if="this.prompts[questionNumber]">
        {{ `${questionNumber + 1}. ${this.prompts[questionNumber]}` }}
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
      this.quizStarted = true;
    },
  },
};
/* cognito blue: #00B3AB
orange: #D85427 #b32a00
gray: #D3DCE4
header darker blue: #234652
lime green: #AED136
*/
</script>
<style scoped>
.container {
}
.answerChoices {
  padding: 20px;
}
button {
  max-width: 40%;

  font-size: 24px;
  border-radius: 14px;
  background-color: #aed136;
  color: white;
  border: 2px solid #234652;
  opacity: 0.6;
  font-family: "Newsreader", serif;
  padding: 10px 10px 10px 10px;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  transition-duration: 0.4s;
}
button:hover {
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
</style>

<style>
body {
  margin-right: 8%;
  margin-left: 8%;
  text-align: center;
  font-family: "Newsreader", serif;
  font-size: 24px;
  color: #234652;
}
</style>
