<template>
  <div>
    <h1>Quiz App</h1>
    <button v-if="!quizStarted" @click="start()">Start quiz</button>
    <div v-if="quizStarted">
      <div className="question">
        {{ this.prompts[questionNumber] }}
      </div>
      <div className="answerChoices">
        <li v-for="choices in answerChoices[questionNumber]">
          {{ choices }}
        </li>
      </div>
      <button @click="onClick()">
        Submit Answer
      </button>
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
      prompts: [],
      answerChoices: [],
      choice: "",
      answers: [],
      answeredBool: false,
      quizStarted: false,
      questionNumber: 0,
    };
  },
  methods: {
    async setQuestionsAndAnswers() {
      this.prompts = this.questions.map((prompt) => prompt.text);
      this.answerChoices = this.questions.map((choices) => choices.answers);
    },
    start() {
      this.quizStarted = true;
      this.setQuestionsAndAnswers();
    },
    onClick() {
      this.answeredBool = true;
      if (!this.answeredBool) return;
      this.questionNumber++;
      this.answers = [...this.answers] + this.choice; // TODO plus selected
      if (this.prompts.length == this.answers.length) this.showSummary();
    },
    // Displays all questions and their respective answers, and the user's answers.
    /* The questions provided are subjective and not all of them have a correct 
    answer (nor is one provided), so we don't calculate a score based on accuracy*/
    showSummary() {
      // questions
      // answers
      // users answers
    },
  },
};
</script>
<style scoped>
button:focus {
  background: greenyellow;
}
</style>
