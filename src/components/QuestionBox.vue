<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>
      <hr class="my-4"/>

      <b-list-group>
        <b-list-group-item 
        v-for="(answer,index) in suffledAnswers" 
        :key="index" 
        @click.prevent="selectIndex(index)"
        :class="answerClass(index)" >
          {{answer}} 
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="this.selectedIndex === null || this.submited ">Submit</b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from "lodash";
  export default {
    
    props:{
      currentQuestion:Object,
      next:Function,
      increment:Function
    },
    data(){
      return {
        selectedIndex:null,
        correctIndex:null,
        suffledAnswers:[],
        submited:false
      }
    },
    watch:{
      currentQuestion:{
        immediate:true,
        handler(){
          this.selectedIndex=null;
          this.submited=false;
          this.suffleAnswers();
        }
      }
    },
    methods:{
      selectIndex(i){
        this.selectedIndex=i
      },
      submitAnswer(){
        let isCorrect=false;
        if(this.selectedIndex === this.correctIndex){
          isCorrect=true;
        }
        this.increment(isCorrect);
        this.submited=true;
      },
      suffleAnswers(){
        let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
        this.suffledAnswers=_.shuffle(answers);
        this.correctIndex=this.suffledAnswers.indexOf(this.currentQuestion.correct_answer);
      },
      answerClass(index){
        let cls='';
        if(this.selectedIndex === index && !this.submited){
          cls='selected';
        }
        else if(index===this.correctIndex && this.submited){
          cls='correct';
        }
        else if(index!==this.correctIndex && this.submited && this.selectedIndex===index){
          cls='incorrect';
        }
        return cls;
      }
    },
    computed:{
      answers(){
        let answers=_.shuffle([...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]);
        return answers;
      }
    }
  }
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
}
.list-group-item:hover{
  background: #eee;
  cursor: pointer;
}
.btn{
  margin: 0 5px;
}

.selected{
  background-color: #7272e0;
}
.correct{
  background-color: #5dbb5d;
}
.incorrect{
  background-color: #e83c3c;
}
</style>