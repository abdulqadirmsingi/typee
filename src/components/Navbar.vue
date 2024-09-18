<template>
  <nav 
    class="p-4 sticky top-0 z-50 transition-all duration-300"
    :class="[
      isScrolled ? 'bg-white bg-opacity-80 backdrop-blur-md shadow-md' : 'bg-white',
      isMenuOpen ? 'bg-white' : ''
    ]"
  >
    <div class="container mx-auto">
      <div class="flex justify-between items-center">
        <!-- Logo/Title -->
        <div class="text-3xl sm:text-4xl font-extrabold text-black transition-all duration-300 hover:text-blue-500">
          <i class="pi pi-keyboard mr-2"></i>
          <span class="bg-clip-text text-transparent bg-gradient-to-r from-blue-500 to-cyan-400">
            Typee..
          </span>
        </div>

        <!-- Hamburger Menu for Mobile -->
        <div class="sm:hidden">
          <button @click="toggleMenu" class="text-gray-600 hover:text-blue-500 focus:outline-none">
            <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path v-if="!isMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
              <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>

        <!-- Navbar Links and Button for Desktop -->
        <div class="hidden sm:flex items-center space-x-6">
          <div class="space-x-4 text-gray-600">
            <a href="https://www.abdulqadirmsingi.com/" target="_blank" class="hover:text-blue-500 transition duration-300 hover:underline">Contact</a>
          </div>

          <button class="bg-gradient-to-r from-blue-500 to-cyan-400 text-white font-semibold px-6 py-2 rounded-full hover:shadow-lg transition-all duration-300 transform hover:scale-105">
            Start Typing
          </button>
        </div>
      </div>

      <!-- Mobile Menu -->
      <div v-if="isMenuOpen" class="sm:hidden mt-4 space-y-4">
        <a href="#" class="block text-gray-600 hover:text-blue-500 transition duration-300">Typing Tests</a>
        <a href="#" class="block text-gray-600 hover:text-blue-500 transition duration-300">Leaderboard</a>
        <a href="#" class="block text-gray-600 hover:text-blue-500 transition duration-300">Lessons</a>
        <a href="#" class="block text-gray-600 hover:text-blue-500 transition duration-300">Contact</a>
        <button class="w-full bg-gradient-to-r from-blue-500 to-cyan-400 text-white font-semibold px-6 py-2 rounded-full hover:shadow-lg transition-all duration-300 transform hover:scale-105">
          Start Typing
        </button>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      isScrolled: false,
      isMenuOpen: false
    };
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll);
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll() {
      this.isScrolled = window.scrollY > 0;
    },
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    }
  }
};
</script>

<style scoped>
@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-5px);
  }
  100% {
    transform: translateY(0px);
  }
}

.pi-keyboard {
  display: inline-block;
  animation: float 3s ease-in-out infinite;
}
</style>