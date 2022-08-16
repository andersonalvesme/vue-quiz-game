<template>
    <ScoreBoard :winCount="winCount" :loseCount="loseCount"/>

    <template v-if="question">
        <h1 v-html="question"></h1>

        <template v-for="(answer, index) in answerOptions" :key="index">
            <input :id="index" v-model="answerSelected" :checked="answer === answerSelected"
                   :disabled="this.answerSubmitted" :value="answer"
                   name="options" type="radio">
            <label :for="index" v-html="answer"></label>
            <br>
        </template>

        <br>
        <button v-if="!this.answerSubmitted" @click="answerSubmit()">Send</button>

        <section v-if="this.answerSubmitted">
            <h4 v-if="this.answerSelected === this.answerCorrect">Correct.</h4>
            <h4 v-if="this.answerSelected !== this.answerCorrect">Wrong. The correct answer is
                "{{ this.answerCorrect }}"</h4>
            <br>
            <button @click="nextQuestion()">Next question</button>
        </section>
    </template>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard";

export default {
    name: 'QuizView',
    components: {
        ScoreBoard
    },
    data() {
        return {
            question: undefined,
            answersIncorrect: undefined,
            answerCorrect: undefined,
            answerSelected: undefined,
            answerSubmitted: false,
            winCount: 0,
            loseCount: 0
        }
    },
    computed: {
        answerOptions() {
            let answers = Object.assign([], this.answersIncorrect)
            answers.splice(Math.round(Math.random() * answers.length), 0, this.answerCorrect)
            return answers
        }
    },
    methods: {
        answerSubmit() {
            if (!this.answerSelected) {
                alert('Pick one of the options')
                return
            }

            this.answerSubmitted = true

            if (this.answerSelected !== this.answerCorrect) {
                this.loseCount++
                return
            }

            this.winCount++
        },
        nextQuestion() {
            this.question = undefined
            this.answersIncorrect = undefined
            this.answerCorrect = undefined
            this.answerSelected = undefined
            this.answerSubmitted = false

            this.axios
                .get('https://opentdb.com/api.php?amount=1')
                .then((result) => {
                    let data = result.data.results[0]

                    this.question = data.question
                    this.answersIncorrect = Object.assign([], data.incorrect_answers)
                    this.answerCorrect = data.correct_answer
                })
        }
    },
    created() {
        this.nextQuestion()
    }
}
</script>

<style lang="scss">
input {
    margin: 12px 4px;
}

button {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
}
</style>