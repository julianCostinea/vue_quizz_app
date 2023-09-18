<template>
  <div id="app">
    <HeaderComponent />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :nextQuestion="nextQuestion" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>

import HeaderComponent from './components/HeaderComponent.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
    HeaderComponent,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0
    }
  },
  methods: {
    nextQuestion() {
      this.index++;
    }
  },
  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&category=18&difficulty=easy&type=multiple')
      .then(response => response.json())
      .then(data => this.questions = data.results);
  }
}
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
