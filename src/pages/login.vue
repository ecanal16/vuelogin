<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import useAuth from '../composables/useAuth';
import useError from "../composables/useError";
const { isAuthenticated, login } = useAuth();
const username = ref("");
const password = ref("");

const router = useRouter();

const loggingIn = () => {
  login(username.value, password.value);
  if (isAuthenticated.value) {
    router.push("/");
  } else {
    setError("Invalid username or password");
    start();
  }
};

const { error, setError} = useError();

import { useTimeout, promiseTimeout } from "@vueuse/core";
const { ready, start } = useTimeout(3000, { controls: true });
</script>


<template>
<div class="flex flex-col items-center justify-center  min-h-screen-nonav">
  Logged in: {{ isAuthenticated }}
    <div class="flex rounded-lg shadow-2xl bg-gray-200 overflow-hidden">
      <img class="h-64" src="../assets/bglogin.png" alt="hello background">
      <form @submit.prevent="loggingIn" class="flex flex-col p-4 space-y-4">
          <input type="text" class="border-2 p-2 rounded-lg" placeholder="Username" v-model="username">
          <input type="password" class="border-2 p-2 rounded-lg" placeholder="Password" v-model="password">
          <button @submit.prevent="loggingIn" type="submit" class="py-2 text-indigo-200 bg-indigo-500 rounded-lg">Login</button>
      </form> 
    </div>
    <div v-if="!ready && error" class="absolute w-1/3 p-4 text-center text-red-800 bg-red-200 rounded-lg  bottom-2 right-2">{{ error }}</div>
</div>
</template>