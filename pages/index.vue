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
