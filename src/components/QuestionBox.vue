<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
      {{ currentQuestion.question }}
      </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item
        v-for="( answer , index) in shuffledAnswers" 
        :key="index"
        @click.prevent="selectedAnswer(index)"
        :class="answerClass(index)"
      >{{ answer }}
      </b-list-group-item>
    </b-list-group>

    <b-button 
      variant="primary" 
      @click="submitAnswer"
      :disabled="selectedIndex === null || answerred"
    >Submit
    </b-button>
    <b-button @click="next">Next</b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props:{
    currentQuestion : Object,
    next : Function,
    increment : Function,
    
  },
  data(){
    return{
      selectedIndex : null,
      shuffledAnswers : [] ,
      correctIndex : null ,
      answerred : false
    }
  },
  methods : {
    selectedAnswer(index){
      return this.selectedIndex = index
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }

      this.answerred = true
      this.increment(isCorrect)
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers , this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answer = "" ;
      if(!this.answerred && this.selectedIndex === index){
         answer = 'selected'
      }else if(this.answerred && this.correctIndex === index){
        answer = 'correct'
      }else if(this.answerred && this.selectedIndex === index && this.correctIndex !== index){
         answer = 'incorrect'
      }

      return answer 
    }
  },
  computed : {
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
   },
   watch: {
     currentQuestion : {
       immediate : true ,
       handler(){
         this.selectedIndex = null 
         this.shuffleAnswers()
         this.answerred = false
       }
     }
     
     
    //  (){
    //    this.selectedIndex = null 
    //    this.shuffleAnswers()
    //  }
   }
}
</script>

<style scoped>
  .list-group{
    margin-bottom: 15px
  }
  .list-group-item:hover{
    background-color: #EEE ;
    cursor: pointer
  }
  .correct{
    background-color: lightgreen;
  }
  .incorrect{
    background-color: red;
  }
  .selected{
    background-color: lightblue ;
  }
  .btn{
    margin: 0 5px
  }
</style>
