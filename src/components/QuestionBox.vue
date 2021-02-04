<template>
    <div class="question-box-container">
        <b-jumbotron>
        
            <template #lead>
            {{ currentQuestion.question }} 
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key= "index"
                    @click.prevent="selectAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                    >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button   variant="primary"
                         @click="submitAnswer"   
        answered: false
                         :disabled="selectedIndex= null || answered"                 
            >
                Submit 
                </b-button>
            <b-button  @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object.prototype,
        next: Function,
        increment: Function
    }, 
    data() {
        return {
        selectedIndex : null,
        shuffledAnswers : [],
        correctIndex: null,
        answered: false

        }

    },
   
    computed: {
        answers: function(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers  
        }
    },
    watch: {
        currentQuestion: {
            immediate : true,
            handler: function(){
                this.selectedIndex = null
                this.shuffleAnswers()
                this.answered = false
            }
        }
    },

    methods:{
        selectAnswer: function(index){
            this.selectedIndex = index
            
        },
        submitAnswer: function(){
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true 
            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex= this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }     
    }
}
</script>

<style scoped>
    .list-group{
        margin-bottom: 15px;
    }
    .btn{
        margin: 0 5px;
    }
    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }
    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: red;
    }

</style>>

