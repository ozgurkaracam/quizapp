<template>
  <div id="app">
    <Header :totalCounter="totalCounter" :correctCount="correctCount"  />
    <Question :currentQuestion="currentQuestion" v-on:correctCount="correct" :next="next" />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Question from './components/QuestionBox.vue';
import axios from 'axios';
export default {
  name: 'app',
  data(){
    return {
      questions:[],
      currentQuestion:Object,
      index:0,
      totalCounter:1,
      correctCount:0
    }
  },
  components: {
    Header,
    Question
  },
  methods:{
    next(){
      this.index++;
      this.totalCounter=this.index+1;
    },
    correct(value){
      this.correctCount=value
    }
  },
  mounted() {
      axios.get('https://opentdb.com/api.php?amount=10').then(
              res=>{this.questions=res.data.results; return this.questions}
      ).then(
              data=>{this.currentQuestion=data[this.index]}
      )
  },
  watch: {
    index(){
      this.currentQuestion=this.questions[this.index];
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
