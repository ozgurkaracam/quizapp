<template>
    <div>
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <p>
                <b-list-group-item v-for="(answer,index) in shuffledAnswers"
                                   :key="index" @click="select(answer)"
                                   :class="{ 'active' : selectedAnswer==answer && !answered ,
                                             'true' : answered && answer==currentQuestion.correct_answer,
                                              'false': answered && answer!=currentQuestion.correct_answer && selectedAnswer==answer}"
                >{{ answer }}</b-list-group-item>

            </p>
            <div v-if="corrected!=null">
                Your answer is <span :style="{ 'color:green':corrected==true,'color:red':corrected==false }"><b>{{ corrected==true ? 'CORRECT' : 'INCORRECT'}}</b></span>
            </div>
            <b-button variant="primary" href="#" style="margin-right: 3px" :disabled="selectedAnswer==null || answered" @click.prevent="answerQuestion">Submit</b-button>
            <b-button variant="success" href="#" @click="next" >Next</b-button>
        </b-jumbotron>
    </div>
</template>
<script>
    import _ from 'lodash';
    export default {
        methods: {
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
        answerQuestion(){
            this.answered=true;
            this.corrected = this.selectedAnswer==this.currentQuestion.correct_answer;
        },
            select(question){
                this.selectedAnswer=question
            }

        },
        data() {
            return {
                shuffledAnswers: [],
                selectedAnswer: null,
                answered:false,
                corrected:null,
                nextt:true
            }
        },
        props: {
            currentQuestion: Object,
            next: Function
        },
        computed: {
            answers() {
                // this function is no longer used in finished code
                // it is replaced by the watch function below and the
                // shuffleAnswers method
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.shuffleAnswers();
                    this.answered=false;
                    this.corrected=null;
                }
            },


        }
    }
</script>
<style scoped>
    .list-group-item:hover{
        cursor:pointer;
        background-color: lightgray;
    }
    .selected{
        background-color:lightblue;
    }
    .true{
        background-color:lightgreen;
    }
    .false{
        background-color:red;
    }
</style>