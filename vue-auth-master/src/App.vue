<script setup lang="ts">
  import { useRouter } from 'vue-router';
  import { ref } from 'vue';
  import { RouterLink, RouterView } from 'vue-router';
  import { useAuthStore } from './stores/auth';

  const auth = useAuthStore();
  const router = useRouter();
  const darkMode = ref(false);

  const handleLogout = () => {
    auth.logout();
    router.push('/login');
  };

  const showAlert = () => {
    alert("You have to be logged in to access this page!");
  };

  const toggleDarkMode = () => {
    darkMode.value = !darkMode.value;
  };
</script>

<template>
  <div class="w-full h-[100vh] px-2" :class="{ 'dark': darkMode }">
    <!-- Navbar -->
    <div class="w-full flex p-4 align-middle justify-between items-center bg-[#55065c] font-outfit font-[400] text-[18px] text-[#d0e38a]" style="border-radius: 12px;">
      <div class="flex space-x-5">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink v-if="auth.username" to="/restricted">Restricted Page</RouterLink>
        <p class="cursor-pointer" v-else @click="showAlert()">Restricted Page</p>
      </div>
      <div class="flex space-x-5 items-center">
        <p v-if="auth.username" class="text-[#d0e38a]">Hi, {{ auth.username }}</p>
        <div v-if="auth.username">
          <button to="/login" class="bg-black p-2 text-[16px] rounded-md" @click="handleLogout()">Logout</button>
        </div>
        <div v-else>
          <RouterLink to="/login" class="bg-black p-2 text-[16px] rounded-md">Login</RouterLink>
        </div>
      </div>
      <!-- Tombol untuk toggle mode malam dan terang -->
      <button @click="toggleDarkMode" class="text-[#d0e38a]">Dark Mode</button>
    </div>
    <RouterView />
  </div>
</template>

<style scoped>
  .dark {
    background-color: #1a202c; /* Warna latar belakang mode malam */
    color: #ffffff; /* Warna teks mode malam */
  }
</style>
