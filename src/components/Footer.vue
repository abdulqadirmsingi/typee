<template>
  <footer class="bg-black text-white py-8">
    <div class="container mx-auto px-4">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <!-- Website Name -->
        <div class="mb-4 md:mb-0">
          <h2 class="text-2xl font-bold">Typee..</h2>
        </div>

        <!-- Navigation Links -->
        <nav class="mb-4 md:mb-0">
          <ul class="flex flex-wrap justify-center md:justify-end space-x-4">
            <li><a href="#" class="hover:text-gray-300 transition duration-300">Home</a></li>
            <li><a href="#" class="hover:text-gray-300 transition duration-300">About</a></li>
            <li><a href="#" class="hover:text-gray-300 transition duration-300">Services</a></li>
            <li><a href="#" class="hover:text-gray-300 transition duration-300">Contact</a></li>
          </ul>
        </nav>

        <!-- Social Media Links -->
        <div class="flex space-x-4">
          <a href="https://x.com/qadirmsingi?s=11" target="_blank" class="hover:text-gray-300 transition duration-300" aria-label="Twitter">
            <i class="pi pi-twitter"></i>
          </a>
          <a href="https://www.linkedin.com/in/abdulqadir-msingi-3b1655276/?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=ios_app" target="_blank" class="hover:text-gray-300 transition duration-300" aria-label="LinkedIn">
            <i class="pi pi-linkedin"></i>
          </a>
          <a href="https://github.com/abdulqadirmsingi" target="_blank" class="hover:text-gray-300 transition duration-300" aria-label="GitHub">
            <i class="pi pi-github"></i>
          </a>
        </div>
      </div>

      <!-- Horizontal Line -->
      <hr class="my-6 border-gray-700">

      <!-- Copyright and Portfolio Link -->
      <div class="flex flex-col md:flex-row justify-between items-center text-sm">
        <p>&copy; {{ new Date().getFullYear() }} Typee.. All rights reserved.</p>
        <p class="mt-2 md:mt-0">
          Designed by 
          <a href="https://www.abdulqadirmsingi.com/" target="_blank" rel="noopener noreferrer" 
             class="font-semibold hover:text-blue-300 transition duration-300">Abdulqadir Msingi</a>
        </p>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style scoped>
/* Add any additional styles here if needed */
</style>