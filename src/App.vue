<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col sm="6" offset="3 ">
          <Header
            :numCorrect="numCorrect"
            :numTotal="numTotal"
           />
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect:0,
      numTotal:0
    };
  },
  methods: {
    increment(isCorrect){
      this.numTotal++
      if(isCorrect){
        this.numCorrect++
      }
    },
    next() {
      this.index++;
      if (this.index > 9) {
        this.index = 0;
      }
    },
  },
  mounted: function () {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=27&difficulty=hard&type=multiple",
      {
        method: "get",
      }
    )
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
