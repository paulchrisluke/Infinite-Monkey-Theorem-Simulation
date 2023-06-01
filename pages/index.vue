<template>
  <div class="container mx-auto px-4 py-8">
    <ScrollingMarquee />

    <h1 class="text-3xl font-bold mb-8">Infinite Monkey Theorem Experiment</h1>

    <div class="flex flex-col lg:flex-row">
      <div class="lg:w-2/3 pr-4">
        <!-- Left column content -->
        <h2 class="text-2xl font-bold mb-4">Experiment: </h2>
        <div>
          <p class="mb-2">Target Text:</p>
          <p class="text-lg font-semibold mb-4">{{ targetText }}</p>
          <p class="mb-2">Attempts: {{ attempts }}</p>
          <p class="mb-2">Elapsed Time: {{ elapsedTime }}</p>
          <div class="mb-4">
            <p>Generated Text:</p>
            <div>{{ generatedText }}</div>
            <div>Matching Score: {{ calculateMatchingScore() }}%</div>
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
        <img src="https://media4.giphy.com/media/pFwRzOLfuGHok/giphy.gif?cid=ecf05e47lr2crdpm9pz7ak2ejbewhhh9pb4j2atmyh7r4guk&ep=v1_gifs_search&rid=giphy.gif&ct=g" alt="Monkey Typing" />
        <div class="mt-4 p-4 bg-gray-100 rounded-lg">
          <p class="text-lg font-semibold mb-2">Highest Scoring Attempt:</p>
          <div class="font-mono" v-if="highestScoringAttempt.generatedText">
            <span v-for="(char, index) in highestScoringAttempt.generatedText" :key="index" :style="{ background: char === targetText.charAt(index) ? 'lightgreen' : 'transparent' }">
              {{ char }}
            </span>
          </div>
          <p class="text-sm mt-2">Matching Score: {{ highestScoringAttempt.matchingScore }}%</p>
          <p class="text-sm mt-1">Attempt Number: {{ highestScoringAttempt.attemptNumber }}</p>
        </div>
      </div>
    </div>

    <AcademicPaper />
    <Footer />
  </div>
</template>

<script>
import AcademicPaper from "@/components/AcademicPaper.vue";
import ScrollingMarquee from "@/components/ScrollingMarquee.vue";
import Footer from "@/components/Footer.vue";


export default {
  components: {
    AcademicPaper,
  },
  data() {
    return {
      targetText: "To be or not to be, that is the question.",
      generatedText: "",
      attempts: 0,
      targetFound: false,
      simulationRunning: false,
      startTime: null,
      elapsedTime: 0,
      intervalId: null,
      highestScoringAttempt: {
        attemptNumber: 0,
        generatedText: "",
        matchingScore: 0,
      },
    };
  },
  methods: {
    generateRandomText(length) {
      const characters =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+.,?'\"/:;[]{}|-=<>`~ ";
      let randomText = "";
      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length);
        randomText += characters.charAt(randomIndex);
      }
      return randomText;
    },
    calculateMatchingScore() {
      const targetText = this.targetText.toLowerCase();
      const generatedText = this.generatedText.toLowerCase();
      const targetLength = targetText.length;
      const generatedLength = generatedText.length;
      let matchingCount = 0;

      for (let i = 0; i < targetLength; i++) {
        if (generatedText[i] === targetText[i]) {
          matchingCount++;
        }
      }

      const matchingScore = (matchingCount / targetLength) * 100;

      return matchingScore.toFixed(2); // Round to two decimal places
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
          const matchingScore = this.calculateMatchingScore();
          if (this.generatedText === this.targetText) {
            this.targetFound = true;
            this.simulationRunning = false;
            clearInterval(this.intervalId);
          }
          if (matchingScore > this.highestScoringAttempt.matchingScore) {
            this.highestScoringAttempt = {
              attemptNumber: this.attempts,
              generatedText: this.generatedText,
              matchingScore: matchingScore,
            };
          }
        }, 10);
      }
    },
    stopSimulation() {
      clearInterval(this.intervalId);
      this.simulationRunning = false;
    },
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  },
};
</script>