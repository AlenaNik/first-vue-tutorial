<template>
        <div className="">
                <b-jumbotron text-variant="dark" border-variant="white">
                        <template slot="lead">
                               {{ currentQuestion.question }}
                        </template>
                        <hr class="my-4">
                    <b-list-group>
                        <b-list-group-item
                                v-for="(answer, index) in answers"
                                :key="index"
                                @click.prevent="selectAnswers(index)"
                                :class="[!answered && selectedIndex === index ? 'selected' :
                                answered && correctIndex === index ? 'correct' :
                                answered && selectedIndex === index
                                && correctIndex !== index ? 'incorrect' : ''

                                ]"
                        >
                            {{ answer }}
                        </b-list-group-item>
                    </b-list-group>


                    <b-button
                            variant="primary"
                            @click="submitAnswer"
                            :disabled="selectedIndex === null || answered"
                    >
                        Submit
                    </b-button>
                    <b-button @click="next" variant="success">
                        Next
                    </b-button>
                </b-jumbotron>
        </div>
</template>
<script>
    import _ from 'lodash';
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
                answered: false
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
                handler() {
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectAnswers(index) {
                this.selectedIndex = index;
            },
            submitAnswer() {
                let isCorrect = false
                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true
                this.increment(isCorrect)
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffleAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffleAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            answerClass(index) {
                let answerClass = ''
                if (!this.answered && this.selectedIndex === index) {
                    answerClass = 'selected'
                } else if (this.answered && this.correctIndex === index) {
                    answerClass = 'correct'
                } else if (this.answered &&
                    this.selectedIndex === index &&
                    this.correctIndex !== index
                ) {
                    answerClass = 'incorrect'
                }
                return answerClass
            }
        }
    }
</script>

<style scoped>
    .jumbotron {
        background-image: linear-gradient(to right, #74ebd5 0%, #9face6 100%)!important;
        border-radius: 6px;
    }
    .list-group {
        margin-bottom: 15px;
    }
    .list-group-item:hover {
        background-color: #d6fff7;
        cursor: pointer;
    }
    .selected {
        background-color: gold;
    }
    .correct {
        background-color: springgreen;
    }
    .incorrect {
        background-color: hotpink;
    }
    .btn {
        margin: 0 5px;
        border-radius: 6px;
    }
</style>