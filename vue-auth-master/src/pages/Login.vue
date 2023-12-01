<script setup lang="ts">
  import { useRouter } from 'vue-router';
  import { useAuthStore } from '../stores/auth'
  import { ref } from 'vue'
  import axios from 'axios'
  import Swal from 'sweetalert2'

  const auth = useAuthStore()
  const username = ref('')
  const password = ref('')
  const router = useRouter()

  const handleLogin = async () => {

    const response = await axios({
      method: 'post',
      url: 'http://localhost:3000/login',
      data: {
        username: username.value,
        password: password.value
      }
    });

    if (response.data.status === "success") {
      Swal.fire({
          icon: "success",
          title: "Login Successful",
          text: "Welcome back!",
        });
      auth.login(username.value)
      router.push('/')
    }

    if (response.data.msg === "Username and Password cannot be empty") {
      Swal.fire({
        title: 'Error!',
        text: 'Please fill out your Username and Password',
        icon: 'error',
        confirmButtonText: 'OK'
      })
    }

    if (response.data.msg === "Incorrect Username and Password") {
      Swal.fire({
        title: 'Error!',
        text: 'Incorrect Username and Password',
        icon: 'error',
        confirmButtonText: 'Try Again'
      })
    }

    if (response.data.msg === "Incorrect Username") {
      Swal.fire({
        title: 'Error!',
        text: 'Incorrect Username',
        icon: 'error',
        confirmButtonText: 'Try Again'
      })
    }

    if (response.data.msg === "Incorrect Password") {
      Swal.fire({
        title: 'Error!',
        text: 'Incorrect Password',
        icon: 'error',
        confirmButtonText: 'Try Again'
      })
    }
    console.log(response)


    // auth.login(username.value)
    // router.push('/')
  }
</script>

<template>
  <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md bg-white rounded-md py-8 px-6">
    <h1 class="mb-4 text-[20px] font-outfit">Login Page</h1>
      <label for="username" class="block mb-2 text-sm font-medium text-gray-700">Username</label>
      <input v-model="username" id="username" name="username" type="text" class="w-full mb-5 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:border-[#41B883]" autocomplete="off" required />
      <label for="username" class="block mb-2 text-sm font-medium text-gray-700">Password</label>
      <input v-model="password" id="password" name="password" type="password" class="w-full mb-5 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:border-[#41B883]" autocomplete="off" required />
      <button @click="handleLogin()" class="w-full bg-[#41B883] p-2 font-outfit text-white text-[16px] rounded-lg">Login</button>
  </div>
</template>