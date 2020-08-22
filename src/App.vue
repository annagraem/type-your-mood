<template>
  <div id="app">
    <input v-model="userInput" placeholder="How are you feeling today?">
    <p>[Debug] mood = {{ mood }}</p>
  </div>
</template>

<script>
import vader from 'vader-sentiment'

export default {
  name: 'App',
  data: function() {
    return {
      userInput: '',
    }
  },
  computed: {
    mood: function () {
      return vader.SentimentIntensityAnalyzer.polarity_scores(this.userInput).compound;
    }
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
  margin-top: 60px;
}
</style>
