<template>
  <div>
    <div>
      <b-alert show dismissible fade
      v-if="selectedAnswer">
        {{selectedAnswer}}
        <b>&rArr;</b>
      </b-alert>
    </div>
    <b-jumbotron>
      <template slot="lead">
        <div>
          <transition name="fade">
            <p v-show="show" :key="currentQuestion.question">{{currentQuestion.question}}</p>
          </transition>
        </div>
      </template>

      <hr class="my-4" />
      <transition>
        <b-list-group v-if="show">
          <b-list-group-item
            v-for="(answer, i) in listAnswers"
            v-bind:key="i"
            v-on:click="selectedAnswerChoice(i)"
          >{{answer}}</b-list-group-item>
        </b-list-group>
      </transition>
      <b-button variant="primary" href="#">Do Something</b-button>
      <b-button variant="success" href="#" v-on:click="forceRerender()" @click="index">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
// Globally
import Vue from "vue";

export default {
  data() {
    return {
      show: true,
      answers: null,
      selectedAnswer: "",
    };
  },

  computed: {
    listAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      console.log("Correct answer: " + this.currentQuestion.correct_answer);
      for (var i = answers.length - 1; i > 0; i--) {
        const randJ = Math.floor(Math.random() * (i + 1));
        let temp = answers[i];
        answers[i] = answers[randJ];
        answers[randJ] = temp;
      }
      this.answers = answers;
      return answers;
    }
  },

  methods: {
    forceRerender: function() {
      this.show = false;
      setTimeout(
        function() {
          this.show = true;
        }.bind(this),
        600
      );
    },

    selectedAnswerChoice: function(index) {
      let answers = [...this.answers];
          this.selectedAnswer = answers[index];
    }
  },

  props: {
    currentQuestion: Object,
    index: Function
  }
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.selected {
  background-color: blue;
}

.correct {
  background-color: green;
}

.incorrect {
  background-color: red;
}
</style>