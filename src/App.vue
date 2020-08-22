<template>
  <div id="app" class="gradient" v-bind:style="{'--mood': mood}">
    <textarea v-model="userInput" placeholder="How are you feeling today?" spellcheck="false" @input="mixin_autoResize_resize" rows="1"></textarea>
  </div>
</template>

<script>
import vader from 'vader-sentiment'
import mixinAutoResize from "./auto-resize.js";

export default {
  name: 'App',
  mixins: [mixinAutoResize],
  data: function() {
    return {
      userInput: '',
    }
  },
  computed: {
    mood: function () {
      let mood = vader.SentimentIntensityAnalyzer.polarity_scores(this.userInput).compound;
      return (mood + 1) / 2;
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
  min-width: 100%;
  min-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;

  font-family: sans-serif;
  color: var(--text-color);

  position: relative;
  background-image: linear-gradient(to bottom right, rgb(106,133,182), rgb(88, 100, 121));
  z-index: 1;

  --mood: 0.5;
}

#app::before {
  position: absolute;
  content: "";
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: -1;
  background-image: linear-gradient(to bottom right, rgb(250,112,154), rgb(254,225,64));
  transition: opacity 0.5s linear;
  opacity: var(--mood);
}

textarea {
  font-size: 3em;
  text-align: center;
  background: none;
  border: none;
  color: inherit;
  border-bottom: 3px solid var(--border-color);

  width: 80%;
  @media screen and (max-width: 600px) {
    width: 95%;
    height: 100px;
  }

  resize: none;
  overflow: hidden;

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
