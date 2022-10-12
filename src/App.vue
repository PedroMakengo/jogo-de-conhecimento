<template>
  <div>
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

    <template v-if="question">
      <h1 v-html="question"></h1>

      <template v-for="(answer, index) in answers" :key="index">
        <div class="question">
          <label v-html="answer"></label>
          <input :disabled="answerSubmitted" type="radio" name="options" :value="answer" v-model="chosenAnswer" />
        </div>
      </template>

      <button v-if="!this.answerSubmitted" @click="this.submitAnswer" class="send" type="button">
        Send
      </button>

      <section v-if="this.answerSubmitted" class="result">
        <h4 v-if="this.chosenAnswer == this.correctAnswer" v-html="
                '&#9989; Congratulations, the answer is ' +
                this.correctAnswer +
                'correct.'
              "></h4>

        <h4 v-else v-html="
                 '&#10060; I´m sorry, you picked the wrong answer. The correct is' +
                 this.correctAnswer +
                 '.'
               "></h4>

        <button @click="this.getNewQuestion" class="send" type="button">
          Next Question
        </button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard.vue";
export default {
  name: "App",
  components: { ScoreBoard },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
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
      if (!this.chosenAnswer) {
        console.log("Pick one of the options");
      } else {
        if (this.chosenAnswer == this.correctAnswer) {
          this.answerSubmitted = true;
          this.winCount++;
        } else {
          this.answerSubmitted = true;
          this.loseCount++;
        }
      }
    },
    getNewQuestion() {
      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      this.axios
        .get("https://opentdb.com/api.php?amount=1&category=18")
        .then(({ data }) => {
          this.question = data.results[0].question;
          this.incorrectAnswers = data.results[0].incorrect_answers;
          this.correctAnswer = data.results[0].correct_answer;
        });
    },
  },
  created() {
    this.getNewQuestion();
  },
};
// https://opentdb.com/api.php?amount=1&category=18
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: linear-gradient(#00000092, #000000ca), url("@/assets/palavra-jogo.webp");
  background-repeat: no-repeat;
  background-size: 100%;
  background-position: center;
  height: 100vh;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: auto;

  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;

  max-width: 50%;

  h1 {
    border-bottom: 1px solid white;
    padding-block: 1rem;
    color: white;
  }

  .question {
    margin-top: 1rem;
    color: white;
  }

  .result {
    color: white;
  }

  .score {
    display: flex;
    justify-content: space-between;

    border-bottom: 1px solid white;
    padding-bottom: 1rem;

    .question-item {
      background: red !important;

      width: 30%;
      height: 3rem;

      display: flex;
      align-items: center;
      justify-content: center;
    }
  }


}

input[type="radio"] {
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background: #cdcd07;
  border: 1px solid #cdcd07;
  cursor: pointer;

  border-radius: 0.2rem;
  color: black;
}
</style>
