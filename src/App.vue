<template>
  <div id="app" v-bind:style="appStyle">
    <input v-model="userInput" placeholder="How are you feeling today?">
  </div>
</template>

<script>
import vader from 'vader-sentiment'

export default {
  name: 'App',
  data: function() {
    return {
      userInput: '',
      leftBadMood: [106,133,182],
      rightBadMood: [186,200,224],
      leftGoodMood: [250,112,154],
      rightGoodMood: [254,225,64],
    }
  },
  computed: {
    appStyle: function () {
      let mood = vader.SentimentIntensityAnalyzer.polarity_scores(this.userInput).compound;
      mood = (mood + 1) / 2;
      console.log("mood (between 0 and 1) : ", mood);
      let leftColor = [];
      for (let i = 0; i < 3; i++) {
        leftColor[i] = Math.round((1 - mood) * this.leftBadMood[i] + mood * this.leftGoodMood[i]);
      }
      let rightColor = [];
      for (let i = 0; i < 3; i++) {
        rightColor[i] = Math.round((1 - mood) * this.rightBadMood[i] + mood * this.rightGoodMood[i]);
      }

      return { backgroundImage: `linear-gradient(to right, rgb(${leftColor[0]}, ${leftColor[1]}, ${leftColor[2]}) 0%, rgb(${rightColor[0]}, ${rightColor[1]}, ${rightColor[2]}) 100%)` }
    }
  }
}
</script>

<style lang="scss">
:root {
  --text-color: white;
  --placeholder-color: rgba(255, 255, 255, 0.5);
  --border-color: rgba(255, 255, 255, 0.2);
}

html, body {
    height: 100%;
    margin: 0;
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;

  font-family: sans-serif;
  color: var(--text-color);
  background-color: #c3cfe2;
}

input {
  font-size: 3em;
  text-align: center;
  background: none;
  border: none;
  color: inherit;
  border-bottom: 3px solid var(--border-color);

  &:focus {
    border: none;
    outline: none;
    border-bottom: 3px solid var(--text-color);
  }
}

::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: var(--placeholder-color);
  opacity: 1; /* Firefox */
}

::-ms-input-placeholder { /* Microsoft Edge */
  color: var(--placeholder-color);
}
</style>
