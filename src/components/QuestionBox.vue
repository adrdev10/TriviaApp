<template>
  <div>
    <b-jumbotron>
      <template slot="lead">
        <div>
          <transition name="fade">
            <p v-if="show" :key="currentQuestion.question">{{currentQuestion.question}}</p>
          </transition>
        </div>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer,i) in currentQuestion.incorrect_answers"
          v-bind:key="i"
        >{{answer}}</b-list-group-item>
      </b-list-group>

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
      show: true
    };
  },

  methods: {
    forceRerender: function() {
      this.show = false;
      setTimeout(
        function() {
          this.show = true;
        }.bind(this),
        500
      );
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
</style>