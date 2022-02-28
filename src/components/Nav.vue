<script setup>
import { useRouter } from "vue-router";
import useAuth from '../composables/useAuth';
const { isAuthenticated, logout } = useAuth();

const router = useRouter();
const loggingOut = () => {
  logout();
  router.push("/");
};
</script>

<template>
<div class="text-indigo-500 bg-slate-400">
<div class="container mx-auto flex items-center justify-between">
    <h1 class="text-3xl font-thin tracking-tighter">
        Vue<span class="font-normal">FakeAuth</span>
      </h1>
    <nav>
        <ul class="flex space-x-4">
            <router-link to="/"><li class="py-8 px-4 hover:cursor-pointer hover:bg-yellow-300 hover:text-white">Home</li></router-link>
            <router-link to="/about"><li class="py-8 px-4 hover:cursor-pointer hover:bg-yellow-300 hover:text-white">About</li></router-link>
            <router-link v-if="!isAuthenticated" to="/login"><li class="py-8 px-4 hover:cursor-pointer hover:bg-yellow-300 hover:text-white">Login</li></router-link>
            <div v-else class="flex">
            <router-link to="/secret"><li class="py-8 px-4 hover:cursor-pointer hover:bg-yellow-300 hover:text-white">Secret</li></router-link>
            <button @click="loggingOut"><li class="py-8 px-4 hover:cursor-pointer hover:bg-yellow-300 hover:text-white">Logout</li></button>
            </div>
        </ul>
    </nav>
</div>
</div>

</template>