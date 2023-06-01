<template>
  <div class="container mx-auto px-4 py-8">
    <marquee class="bg-gray-100 p-4 rounded-lg mb-4" direction="left" behavior="scroll" scrollamount="8">
      <p class="mb-4" style="display: inline-block; white-space: nowrap;">
        The Infinite Monkey Theorem is a concept in probability theory that suggests an infinite number of monkeys, randomly pressing keys on typewriters, would eventually type out the complete works of Shakespeare or any other given text. According to the theorem, given enough time, the randomness of the monkeys' key presses would eventually lead to the exact reproduction of any given text, including all the words, punctuation, and formatting. While the probability of a monkey randomly typing out a specific text in a finite amount of time is incredibly low, the theorem demonstrates the concept of probability and the potential outcomes over an infinite time frame.
      </p>
    </marquee>

    <h1 class="text-3xl font-bold mb-8">Case Study: Infinite Monkey Theorem</h1>

    <div class="flex flex-col lg:flex-row">
      <div class="lg:w-2/3 pr-4">
        <!-- Left column content -->
        <h2 class="text-2xl font-bold mb-4">Experiment: Infinite Monkey Theorem Simulation</h2>
        <div>
          <p class="mb-2">Target Text:</p>
          <p class="text-lg font-semibold mb-4">{{ targetText }}</p>
          <p class="mb-2">Attempts: {{ attempts }}</p>
          <p class="mb-2">Elapsed Time: {{ elapsedTime }}</p>
          <div class="mb-4">
            <p>Generated Text:</p>
            <div>{{ generatedText }}</div>
          </div>
          <div v-if="!targetFound">
            <button class="px-4 py-2 bg-blue-500 text-white rounded" @click="simulateTheorem">Simulate</button>
            <button class="px-4 py-2 bg-red-500 text-white rounded ml-4" @click="stopSimulation" v-if="simulationRunning">Stop</button>
          </div>
          <div v-else>
            <p class="text-xl font-bold mb-4">Target text '{{ targetText }}' found after {{ attempts }} attempts!</p>
          </div>
        </div>
      </div>
      <div class="lg:w-1/3 mt-8 lg:mt-0">
        <!-- Right column content -->
        <img src="https://i.gifer.com/origin/d6/d66620ccdb4aee4182879a2c07d393ef_w200.gif" alt="Monkey Typing" />
      </div>
    </div>
    <footer class="mt-8 py-4 bg-gray-200 text-center">
      <div class="flex items-center justify-center">
        <a href="https://github.com/paulchrisluke/Infinite-Monkey-Theorem-Simulation.git" aria-label="GitHub" class="footer-octicon" title="GitHub">
          <svg aria-hidden="true" class="octicon octicon-mark-github" height="24" version="1.1" viewBox="0 0 16 16" width="24">
            <path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"></path>
          </svg>
        </a>
        <a class="text-gray-600 hover:text-gray-800 ml-2" href="https://github.com/paulchrisluke/Infinite-Monkey-Theorem-Simulation.git" target="_blank" rel="noopener noreferrer">
          Proudly Open Source
        </a>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      targetText: "To be or not to be, that is the question.",
      generatedText: "",
      attempts: 0,
      targetFound: false,
      simulationRunning: false,
      startTime: null,
      elapsedTime: 0,
      intervalId: null
    };
  },
  methods: {
    generateRandomText(length) {
      const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+.,?'\"/:;[]{}|-=<>`~ ";
      let randomText = "";
      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length);
        randomText += characters.charAt(randomIndex);
      }
      return randomText;
    },
    simulateTheorem() {
      if (!this.simulationRunning) {
        this.targetFound = false;
        this.simulationRunning = true;
        this.startTime = new Date();
        this.intervalId = setInterval(() => {
          const currentTime = new Date();
          this.elapsedTime = Math.floor((currentTime - this.startTime) / 1000);
          this.attempts++;
          this.generatedText = this.generateRandomText(this.targetText.length);
          if (this.generatedText === this.targetText) {
            this.targetFound = true;
            this.simulationRunning = false;
            clearInterval(this.intervalId);
          }
        }, 10);
      }
    },
    stopSimulation() {
      clearInterval(this.intervalId);
      this.simulationRunning = false;
    }
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  }
};
</script>
