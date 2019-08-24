<template>
  <div id="app">
    <Header></Header>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="10" offset="1">
          <QuestionBox v-bind:currentQuestion="questions[index]"
                      v-bind:index="next"
          ></QuestionBox>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0
    };
  },

  methods: {
    next() {
      this.index = (this.index+1) % this.questions.length;
    }
  },
  
  created() {

  },
  mounted() {
    fetch("https://opentdb.com/api.php?amount=20&type=multiple", {
      method: "get"
    })
      .then(resp => {
        return resp.json();
      })
      .then(jdata => {
        this.questions = jdata.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
