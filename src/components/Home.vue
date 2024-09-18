<template>
  <div
    class="min-h-screen bg-gradient-to-r flex flex-col items-center justify-center p-4"
  >
    <div class="text-center mb-8">
      <h1 class="text-5xl font-bold mb-4 text-black">TypeType Challenge</h1>
    </div>

    <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-3xl">
      <!-- Challenge Mode Selector -->
      <div class="mb-6">
        <h2 class="text-2xl font-semibold mb-2">Choose Your Challenge:</h2>
        <div class="flex space-x-4">
          <button
            v-for="mode in challengeModes"
            :key="mode"
            @click="selectMode(mode)"
            :class="[
              'px-4 py-2 rounded-full transition',
              selectedMode === mode
                ? 'bg-blue-500 text-white'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300',
            ]"
          >
            {{ mode }}
          </button>
        </div>
      </div>

      <!-- Completion Message -->
      <div
        v-if="challengeCompleted"
        class="mb-6 p-6 bg-green-100 rounded-lg text-center"
      >
        <h3 class="text-2xl font-bold text-green-700 mb-4">Congratulations!</h3>
        <p class="text-lg text-green-600 mb-4">
          You've completed the typing challenge. Great job!
        </p>
        <div class="grid grid-cols-2 gap-4">
          <div class="bg-white p-3 rounded shadow">
            <p class="font-semibold text-gray-700">Final WPM</p>
            <p class="text-2xl font-bold text-blue-500">{{ wpm }}</p>
          </div>
          <div class="bg-white p-3 rounded shadow">
            <p class="font-semibold text-gray-700">Accuracy</p>
            <p class="text-2xl font-bold text-green-500">{{ accuracy }}%</p>
          </div>
          <div class="bg-white p-3 rounded shadow">
            <p class="font-semibold text-gray-700">Time Taken</p>
            <p class="text-2xl font-bold text-purple-500">{{ timeElapsed }}s</p>
          </div>
          <div class="bg-white p-3 rounded shadow">
            <p class="font-semibold text-gray-700">Characters Typed</p>
            <p class="text-2xl font-bold text-orange-500">
              {{ userInput.length }}
            </p>
          </div>
        </div>
      </div>

      <!-- Random Text Display -->
      <div
        v-if="!challengeCompleted"
        class="relative mb-6 p-4 bg-gray-100 rounded-lg"
      >
        <p class="text-lg leading-relaxed" v-if="challengeStarted">
          <span
            v-for="(char, index) in randomText"
            :key="index"
            :class="{
              'text-green-500':
                index < userInput.length && char === userInput[index],
              'text-red-500':
                index < userInput.length && char !== userInput[index],
              'text-gray-800': index >= userInput.length,
            }"
            >{{ char }}</span
          >
        </p>
        <p v-else class="text-lg text-gray-800 leading-relaxed">
          {{ randomText }}
        </p>
        <div
          class="absolute bottom-0 left-0 h-1 bg-blue-500 transition-all duration-300"
          :style="{ width: `${(userInput.length / randomText.length) * 100}%` }"
        ></div>
      </div>

      <!-- Typing Input -->
      <textarea
        v-if="!challengeCompleted"
        v-model="userInput"
        @input="checkTyping"
        @keydown.enter.prevent
        :disabled="challengeCompleted"
        class="w-full p-3 border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400 mb-4"
        rows="3"
        placeholder="Start typing here..."
      ></textarea>

      <!-- Typing Stats -->
      <div v-if="!challengeCompleted" class="grid grid-cols-2 gap-4 mb-6">
        <div class="bg-gray-100 p-4 rounded-lg text-center">
          <div class="text-3xl font-bold text-blue-500">{{ wpm }}</div>
          <div class="text-sm text-gray-600">Words per minute</div>
        </div>
        <div class="bg-gray-100 p-4 rounded-lg text-center">
          <div class="text-3xl font-bold text-green-500">{{ accuracy }}%</div>
          <div class="text-sm text-gray-600">Accuracy</div>
        </div>
        <div class="bg-gray-100 p-4 rounded-lg text-center">
          <div class="text-3xl font-bold text-purple-500">
            {{ timeElapsed }}s
          </div>
          <div class="text-sm text-gray-600">Time Elapsed</div>
        </div>
        <div class="bg-gray-100 p-4 rounded-lg text-center">
          <div class="text-3xl font-bold text-orange-500">
            {{ realTimeWPM }}
          </div>
          <div class="text-sm text-gray-600">Real-time WPM</div>
        </div>
      </div>

      <!-- Button Controls -->
      <div class="text-center">
        <button
          @click="resetChallenge"
          class="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600 transition"
        >
          {{ challengeStarted ? "Reset" : "Start Challenge" }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      randomText: "",
      userInput: "",
      wpm: 0,
      realTimeWPM: 0,
      accuracy: 100,
      timeElapsed: 0,
      timer: null,
      startTime: null,
      challengeCompleted: false,
      challengeStarted: false,
      selectedMode: "Normal",
      challengeModes: ["Easy", "Normal", "Hard", "Very Hard"],
      textPool: {
        Easy: [
          "The quick brown fox jumps over the lazy dog.",
          "Practice makes perfect.",
          "A journey of a thousand miles begins with a single step.",
          "Typing is an essential skill for everyone.",
          "Time flies when you're having fun.",
          "Success is the result of hard work and determination.",
          "The rain in Spain stays mainly in the plain.",
          "He sells sea shells by the sea shore.",
          "Learning to type efficiently is a valuable skill.",
          "Rome wasn't built in a day, but they worked on it every single day.",
        ],
        Normal: [
          "To be or not to be, that is the question.",
          "All that glitters is not gold.",
          "The only way to do great work is to love what you do.",
          "Actions speak louder than words.",
          "A picture is worth a thousand words.",
          "Success usually comes to those who are too busy to be looking for it.",
          "Good things come to those who wait, but better things come to those who go out and get them.",
          "The greatest glory in living lies not in never falling, but in rising every time we fall.",
          "In the end, it's not the years in your life that count. It's the life in your years.",
          "The future belongs to those who believe in the beauty of their dreams.",
        ],
        Hard: [
          "I am the master of my fate, I am the captain of my soul.",
          "It was the best of times, it was the worst of times.",
          "Two roads diverged in a wood, and I took the one less traveled by.",
          "In three words I can sum up everything I've learned about life: it goes on.",
          "Do not go where the path may lead, go instead where there is no path and leave a trail.",
          "The only limit to our realization of tomorrow is our doubts of today.",
          "In the middle of difficulty lies opportunity.",
          "The only thing we have to fear is fear itself.",
          "I wandered lonely as a cloud that floats on high o'er vales and hills, when all at once I saw a crowd, a host of golden daffodils.",
          "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.",
        ],
        VeryHard: [
          "It was a bright cold day in April, and the clocks were striking thirteen. Winston Smith, his chin nuzzled into his breast in an effort to escape the vile wind, slipped quickly through the glass doors of Victory Mansions, though not quickly enough to prevent a swirl of gritty dust from entering along with him.",
          "All happy families are alike; each unhappy family is unhappy in its own way. Everything was in confusion in the Oblonskys’ house. The wife had discovered that the husband was carrying on an intrigue with a French girl, who had been a governess in their family, and she declared that she could not go on living in the same house with him.",
          "It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair, we had everything before us, we had nothing before us, we were all going direct to Heaven, we were all going direct the other way.",
          "There was no possibility of taking a walk that day. We had been wandering, indeed, in the leafless shrubbery an hour in the morning; but since dinner (Mrs. Reed, when there was no company, dined early) the cold winter wind had brought with it clouds so sombre, and a rain so penetrating, that further outdoor exercise was now out of the question.",
        ],
      },
    };
  },
  methods: {
    generateRandomText() {
      const texts = this.textPool[this.selectedMode];
      const randomIndex = Math.floor(Math.random() * texts.length);
      this.randomText = texts[randomIndex];
    },
    checkTyping() {
      if (!this.challengeStarted) {
        this.startChallenge();
      }

      const wordsTyped = this.userInput.trim().split(" ").length;
      const timeInMinutes = this.timeElapsed / 60;
      this.wpm = Math.round(wordsTyped / timeInMinutes) || 0;

      this.calculateAccuracy();
      this.updateRealTimeWPM();

      if (this.userInput === this.randomText) {
        this.stopChallenge();
      }
    },
    calculateAccuracy() {
      const inputText = this.userInput;
      const originalText = this.randomText.slice(0, inputText.length);
      let correctChars = 0;

      for (let i = 0; i < inputText.length; i++) {
        if (inputText[i] === originalText[i]) {
          correctChars++;
        }
      }

      const accuracy = (correctChars / inputText.length) * 100;
      this.accuracy = isNaN(accuracy) ? 100 : accuracy.toFixed(2);
    },
    updateRealTimeWPM() {
      const timeElapsed = (Date.now() - this.startTime) / 1000 / 60; // in minutes
      const wordsTyped = this.userInput.trim().split(" ").length;
      this.realTimeWPM = Math.round(wordsTyped / timeElapsed) || 0;
    },
    startChallenge() {
      this.challengeStarted = true;
      this.startTime = Date.now();
      this.timer = setInterval(() => {
        this.timeElapsed = ((Date.now() - this.startTime) / 1000).toFixed(0);
        this.updateRealTimeWPM();
      }, 1000);
    },
    stopChallenge() {
      clearInterval(this.timer);
      this.challengeCompleted = true;
    },
    resetChallenge() {
      this.userInput = "";
      this.wpm = 0;
      this.realTimeWPM = 0;
      this.accuracy = 100;
      this.timeElapsed = 0;
      this.challengeCompleted = false;
      this.challengeStarted = false;
      this.startTime = null;
      clearInterval(this.timer);
      this.generateRandomText();
    },
    selectMode(mode) {
      this.selectedMode = mode;
      this.resetChallenge();
    },
  },
  mounted() {
    this.generateRandomText();
  },
};
</script>

<style scoped>
/* Add any component-specific styles here */
</style>
