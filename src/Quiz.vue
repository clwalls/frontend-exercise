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
    <!-- <div>
      {{ displayQuestionsAndAnswers() }}
    </div> -->
    <div v-if="quizStarted">
      <!-- v-if to make sure question at that index is not null/undefined-->
      <div className="question" v-if="this.prompts[questionNumber]">
       {{ `${this.prompts[questionNumber]}` }}
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
    <div className="results" v-if="quizOver">
      <!-- <span v-html="displayResults()"></span> -->
      <!-- {{ showResults() }} -->
      
      <h3>Your answers:</h3>
      <div v-for="(prompt, index) in prompts">
        <div className="prompt">
          {{ prompt }}
        </div>
        <div className="answer" v-for="(answer, ndex) in answerChoices[index]">
          <label class="a-choices">
            <ul>
              <li class="chosen" v-if="answerChoices[index][ndex] == answers[index]">{{answerChoices[index][ndex]}}</li>
              <li v-else>{{answerChoices[index][ndex]}}</li>
            </ul>
          </label>
        </div>
        <br/>
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
  // computed: {
  //   showResults: function () {
  //     return null
  //   }
  // },
  methods: {
    async setQuestionsAndAnswers() {
      this.prompts = this.questions.map((prompt, index) => `${index+1}. ${prompt.text}`);
      this.answerChoices = this.questions.map((choices) => choices.answers);
    },
    start() {
      this.quizStarted = true;
      this.quizOver = false;
      this.questionNumber = 0;
    },
    showResults() {
      let question = [];
      let currQuestion = [];
      let prompt = '';
      for(let i = 0; i < this.prompts.length; i++) {
        prompt = this.prompts[i];
        console.log(prompt)
        currQuestion = [...currQuestion, `${prompt}`]
        for(let j = 0; j < this.answerChoices[i].length; j++) {
          console.log(this.answerChoices[i][j])
          currQuestion = [...currQuestion, ]
        }
      }
      return currQuestion;
    },
      /*
      <div>
      
      <h3>Your answers:</h3>
      <div v-for="(prompt, index) in prompts">
        <div className="prompt">
          {{ `${index+1}. ${prompt}` }}
        </div>
        <div className="answer" v-for="answers in answerChoices[index]">
          <label class="a-choices">
            <ul>
              <li v-if="answers[index] == answerChoices[index]">equals</li>
              <li v-else>not equals</li>
            </ul>
          </label>
        </div>
        <br/>
      </div>
      </div>

      */ 
    submitAnswer() {
      if (this.choice == null) return;
      this.answers = [...this.answers, this.choice]
      console.log(this.answers)
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
.chosen {
  color: #b32a00;
}
.results {
  position: left;
}
.prompt {

}

.answer {
}
/* Customize the label (the container) */

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
