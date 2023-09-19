<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in answers" :key="index" :active="selectedIndex === index"
                    @click="selectAnswer(index)" :class="answerClass(index)">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" @click="submitAnswer"
                :disabled="selectedIndex == null || answered">Submit</b-button>
            <b-button variant="success" @click="nextQuestion">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    name: 'QuestionBox',
    props: {
        currentQuestion: Object,
        nextQuestion: Function,
        increment: Function
    },
    computed: {
        answers() {
            return this.currentQuestion.incorrect_answers.concat(this.currentQuestion.correct_answer);
        }
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    watch: {
        // currentQuestion() {
        //     this.selectedIndex = null;
        //     this.shuffleAnswers();
        // }
        currentQuestion: {
            handler() {
                this.selectedIndex = null;
                this.shuffleAnswers();
                this.answered = false;
            },
            immediate: true
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
        },
        shuffle(array) {
            var currentIndex = array.length, temporaryValue, randomIndex;

            // While there remain elements to shuffle...
            while (0 !== currentIndex) {

                // Pick a remaining element...
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;

                // And swap it with the current element.
                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;

            }

            return array;
        },
        shuffleAnswers() {
            this.shuffledAnswers = this.shuffle(this.answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false;
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        answerClass(index) {
            if (this.answered) {
                if (index === this.correctIndex) {
                    return 'correct';
                } else if (index === this.selectedIndex) {
                    return 'incorrect';
                }
            } else {
                if (index === this.selectedIndex) {
                    return 'selected';
                }
            }
        }
    }
}
</script>

<style scoped>
.question-box-container {
    margin-top: 20px;
}

.btn {
    margin-right: 10px;
    margin-top: 10px;
}

.list-group-item:hover {
    background-color: #f5f5f5;
    cursor: pointer;
}

.correct {
    background-color: #28a745 !important;
    color: #fff;
}

.incorrect {
    background-color: #dc3545 !important;
    color: #fff;
}

.selected {
    background-color: #007bff !important;
    color: #fff;
}
</style>
