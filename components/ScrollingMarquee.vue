<template>
    <marquee class="bg-gray-100 p-4 rounded-lg mb-4" direction="left" behavior="scroll" scrollamount="8">
      <p class="mb-4" style="display: inline-block; white-space: nowrap;">
        {{ marqueeText }}
      </p>
    </marquee>
  </template>
  
  <script>
  export default {
    data() {
      return {
        marqueeText:
          "The Infinite Monkey Theorem is a concept in probability theory that suggests an infinite number of monkeys, randomly pressing keys on typewriters, would eventually type out the complete works of Shakespeare or any other given text. According to the theorem, given enough time, the randomness of the monkeys' key presses would eventually lead to the exact reproduction of any given text, including all the words, punctuation, and formatting. While the probability of a monkey randomly typing out a specific text in a finite amount of time is incredibly low, the theorem demonstrates the concept of probability and the potential outcomes over an infinite time frame.",
      };
    },
  };
  </script>
  
  <style scoped>
  /* Add any necessary styles for the marquee component */
  </style>
  