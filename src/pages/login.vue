<script setup>
import { useRouter } from "vue-router";

import { useForm, useField} from 'vee-validate';
import * as yup from 'yup';

const schema = yup.object({
  username: yup.string().required().email().label("Email"),
  password: yup.string().required().min(8).label("Password"),
});

useForm({
  validationSchema: schema,
});

const { value: username, errorMessage: emailError } = useField("username");
const { value: password, errorMessage: passwordError } = useField("password");

import useAuth from "../composables/useAuth";
import useError from "../composables/useError";
const { isAuthenticated, login, signup, googleLogin } = useAuth();


const router = useRouter();

const loggingIn = async () => {
  await login(username.value, password.value);
  goToHome();
};

const signingUp = async () => {
  await signup(username.value, password.value);
  goToHome();
};

const google = async () => {
  await googleLogin();
  goToHome();
};

const goToHome = () => {
  if (isAuthenticated.value) {
    router.push("/");
  } else {
    setError("Invalid username or password");
    start();
  }
};

const { error, setError } = useError();

import { useTimeout, promiseTimeout } from "@vueuse/core";
const { ready, start } = useTimeout(3000, { controls: true });
</script>


<template>
<div class="flex flex-col items-center justify-center  min-h-screen-nonav">
    <div class="flex rounded-lg shadow-2xl bg-blue-600 overflow-hidden">
      <img class="h-64" src="../assets/bglogin.png" alt="hello background">
      <form @submit.prevent="loggingIn" class="flex flex-col p-4 space-y-4">
          <input name="username" type="text" class="border-2 p-2 rounded-lg" placeholder="Email" v-model="username">
          <span class="text-xs text-center bg-red-200 text-red-500">{{ emailError }}</span>
          <input name="password" type="password" class="border-2 p-2 rounded-lg" placeholder="Password" v-model="password">
          <span class="text-xs text-center bg-red-200 text-red-500">{{ passwordError }}</span>
          <div class="flex space-x-2">
            <button @submit.prevent="loggingIn" type="submit" class="w-1/2 py-2 text-indigo-200 bg-indigo-500 rounded-lg">Login</button>
            <button @click="signingUp" class="w-1/2 py-2 bg-green-200 rounded-lg">Sign Up</button>
          </div>
            <button @click="google" class="bg-white flex justify-center py-2 rounded-lg hover:bg-blue-900"><img src="https://upload.wikimedia.org/wikipedia/commons/5/53/Google_%22G%22_Logo.svg" alt="google logo"/></button>
      </form> 
    </div>
    <div v-if="!ready && error" class="absolute w-1/3 p-4 text-center text-red-800 bg-red-200 rounded-lg  bottom-2 right-2">{{ error }}</div>
</div>
</template>