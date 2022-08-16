<template>
    <QuizView/>
</template>

<script>
import QuizView from './components/QuizView.vue';

export default {
    name: 'App',
    components: {
        QuizView
    }
}
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    max-width: 960px;
    margin: 60px auto;
}
</style>
