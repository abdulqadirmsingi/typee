<!-- Hero.vue -->
<template>
  <div class="flex items-center justify-center min-h-screen pt-16">
    <div class="text-center px-4">
      <h2 class="text-5xl md:text-7xl font-bold mb-8 leading-tight animate-fade-in-down">
        Most typing games don't
        <span class="text-blue-600">verify speed.</span>
        <br />So we created one that does.
      </h2>
      <p class="text-2xl md:text-3xl mb-12 animate-fade-in-up">Verified Typing, Real Challenges. No Cheating!</p>
      <button class="bg-blue-500 hover:bg-blue-600 text-white text-xl font-bold py-3 px-8 rounded-full transition-all duration-300 transform hover:scale-105 animate-pulse">
        START TYPING NOW
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero'
}
</script>

<style scoped>
@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in-down {
  animation: fadeInDown 1s ease-out;
}

.animate-fade-in-up {
  animation: fadeInUp 1s ease-out 0.5s both;
}
</style>