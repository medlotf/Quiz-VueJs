<template>
  <div id="app">
    <Headerr :correct="correct" :total="total" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :next="next" :increment="increment" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Headerr from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Headerr,
    QuestionBox
  },
  data(){
    return {
      questions:[],
      index:0,
      correct:0,
      total:0
    }
  },
  methods:{
    next(){
      this.index == this.questions.length -1 ? this.index=0 : this.index++;
    },
    increment(isCor){
      if(isCor)
      {
        this.correct++;
      }
      this.total++;
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=21&type=multiple',{method:'get'})
    .then(res=>{
      return res.json();
    })
    .then(jsonData=>{
      this.questions=jsonData.results
    })
  }
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
