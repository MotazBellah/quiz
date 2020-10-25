<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group
                v-for="(answer, index) in shuffledAnswers"
                :key="index"
                @click="selectAnswer(index)"
                :class="[
                    !answered && selectedIndex === index ? 'selected' :
                    answered && correctIndex === index ? 'correct' :
                    answered && selectedIndex === index && correctIndex !== index ? 'incorrect' : ''
                ]"
            >
              <b-list-group-item>{{ answer }}</b-list-group-item>
            </b-list-group>


            <b-button
                variant="primary"
                v-on:click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button @click="next" variant="success">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import  _ from 'lodash'

    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false,
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler(){
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index

            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                console.log(answers);
                this.shuffledAnswers = _.shuffle(answers)

                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer() {
                let isCorrect = false
                console.log(this.selectedIndex);
                console.log(this.correctIndex);
                console.log(this.shuffledAnswers);
                // if (this.shuffledAnswers[this.selectedIndex] === this.currentQuestion.correct_answer) {
                //     isCorrect = true
                // }
                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true
                this.increment(isCorrect)

            }
        },

    };
</script>

<style scoped>
    .list-group {
        /* margin-bottom: 15px; */
    }
    .list-group-item:hover {
        background-color: #EEE;
        cursor: pointer;
    }
    .btn {
        margin: 5px 5px;
    }

    .selected {
        background-color: blue;
        color: blue;
        font-weight: bold;
    }
    .correct {
        background-color: green;
        color: green;
        font-weight: bold;
    }
    .incorrect {
        background-color: red;
        color: red;
        font-weight: bold;
    }
</style>
