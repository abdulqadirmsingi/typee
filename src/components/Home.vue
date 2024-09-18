<template>
  <div
    class="min-h-screen bg-gradient-to-r flex flex-col items-center justify-center p-4"
  >
    <div class="text-center mb-8">
      <h1 class="text-5xl font-extrabold mb-4 text-black">
        Challenge Your Typing Skills here..
      </h1>
    </div>

    <div class="bg-white p-4 sm:p-8 rounded-lg shadow-lg w-full max-w-3xl">
      <!-- Challenge Mode Selector -->
      <div class="mb-6">
        <h2 class="text-2xl font-semibold mb-2">Choose Your Challenge:</h2>
        <div class="flex flex-wrap gap-2">
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
        class="mb-6 p-4 sm:p-6 bg-green-100 rounded-lg text-center"
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
          "Every cloud has a silver lining.",
          "Actions speak louder than words.",
          "Better late than never.",
          "Don't judge a book by its cover.",
          "Easy come, easy go.",
          "Honesty is the best policy.",
          "Laughter is the best medicine.",
          "Where there's a will, there's a way.",
          "You can't have your cake and eat it too.",
          "When in Rome, do as the Romans do.",
        ],
        Normal: [
          "To be or not to be, that is the question.",
          "All that glitters is not gold.",
          "The only way to do great work is to love what you do.",
          "A picture is worth a thousand words.",
          "Success usually comes to those who are too busy to be looking for it.",
          "The greatest glory in living lies not in never falling, but in rising every time we fall.",
          "In the end, it's not the years in your life that count. It's the life in your years.",
          "The future belongs to those who believe in the beauty of their dreams.",
          "Life is what happens to you while you're busy making other plans.",
          "Twenty years from now you will be more disappointed by the things that you didn't do than by the ones you did do.",
          "The two most important days in your life are the day you are born and the day you find out why.",
          "There is no greater agony than bearing an untold story inside you.",
          "Everything you've ever wanted is on the other side of fear.",
          "Success is not final, failure is not fatal: it is the courage to continue that counts.",
          "Believe you can and you're halfway there.",
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
          "I wandered lonely as a cloud that floats on high o'er vales and hills.",
          "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.",
          "Ask not what your country can do for you – ask what you can do for your country.",
          "We shall defend our island, whatever the cost may be, we shall fight on the beaches, we shall fight on the landing grounds, we shall fight in the fields and in the streets, we shall fight in the hills; we shall never surrender.",
          "That's one small step for man, one giant leap for mankind.",
          "I have a dream that one day this nation will rise up and live out the true meaning of its creed.",
          "The greatest challenge to any thinker is stating the problem in a way that will allow a solution.",
        ],
        "Very Hard": [
          "It was a bright cold day in April, and the clocks were striking thirteen. Winston Smith, his chin nuzzled into his breast in an effort to escape the vile wind, slipped quickly through the glass doors of Victory Mansions, though not quickly enough to prevent a swirl of gritty dust from entering along with him.",
          "All happy families are alike; each unhappy family is unhappy in its own way. Everything was in confusion in the Oblonskys' house. The wife had discovered that the husband was carrying on an intrigue with a French girl, who had been a governess in their family, and she declared that she could not go on living in the same house with him.",
          "It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair.",
          "Many years later, as he faced the firing squad, Colonel Aureliano Buendía was to remember that distant afternoon when his father took him to discover ice. At that time Macondo was a village of twenty adobe houses, built on the bank of a river of clear water that ran along a bed of polished stones, which were white and enormous, like prehistoric eggs.",
          "The sky above the port was the color of television, tuned to a dead channel. It's not like I'm using, Rattle thought. It's like my body's developed this massive drug deficiency. He stared at the stars, remembering: the nights he'd spent with her before they were married, their bodies locked together, pale arms and legs tangled on the dark sheets in the purple glow of the city's mingled lights.",
          "In my younger and more vulnerable years my father gave me some advice that I've been turning over in my mind ever since. 'Whenever you feel like criticizing anyone,' he told me, 'just remember that all the people in this world haven't had the advantages that you've had.' He didn't say any more, but we've always been unusually communicative in a reserved way, and I understood that he meant a great deal more than that.",
          "It was a queer, sultry summer, the summer they electrocuted the Rosenbergs, and I didn't know what I was doing in New York. I'm stupid about executions. The idea of being electrocuted makes me sick, and that's all there was to read about in the papers—goggle-eyed headlines staring up at me on every street corner and at the fusty, peanut-smelling mouth of every subway.",
          "Far out in the uncharted backwaters of the unfashionable end of the western spiral arm of the Galaxy lies a small, unregarded yellow sun. Orbiting this at a distance of roughly ninety-two million miles is an utterly insignificant little blue-green planet whose ape-descended life forms are so amazingly primitive that they still think digital watches are a pretty neat idea.",
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
