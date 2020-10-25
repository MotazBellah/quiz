<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group
                v-for="(answer, index) in answers"
                :key="index"
                @click="selectAnswer(index)"
                :class="[selectedIndex === index ? 'selected' : '']"
            >
              <b-list-group-item>{{ answer }}</b-list-group-item>
            </b-list-group>


            <b-button variant="primary" href="#">Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default {
        props: {
            currentQuestion: Object,
            next: Function
        },
        data() {
            return {
                selectedIndex: null
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index

            }
        }
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
    }
    .correct {
        background-color: green;
    }
    .incorrect {
        background-color: red;
    }
</style>
