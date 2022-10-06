<template>
  <div>
    <template v-if="question">
      <h1 v-html="question"></h1>

      <template v-for="(answer, index) in answers" :key="index">
        <input
          type="radio"
          name="options"
          :value="answer"
          v-model="chose_answer"
        />
        <label v-html="answer"></label> <br />
      </template>
      <button @click="submitAnswer" class="send" type="button">Send</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chose_answer: undefined,
    };
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      // Pegando a minha posição
      let positionRandom = Math.round(Math.random() * answers.length);

      answers.splice(positionRandom, 0, this.correctAnswer);
      return answers;
    },
  },
  methods: {
    submitAnswer() {
      if (!this.chose_answer) {
        alert("Escolha uma das opções");
      } else {
        if (this.chose_answer == this.correctAnswer) {
          alert("You got it right!");
        } else {
          alert("You got it wrong!");
        }
      }
    },
  },
  created() {
    this.axios
      .get("https://opentdb.com/api.php?amount=1&category=18")
      .then(({ data }) => {
        this.question = data.results[0].question;
        this.incorrectAnswers = data.results[0].incorrect_answers;
        this.correctAnswer = data.results[0].correct_answer;
      });
  },
};
// https://opentdb.com/api.php?amount=1&category=18
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;

  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
