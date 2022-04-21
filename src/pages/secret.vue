<script setup>
import { onUnmounted, ref } from 'vue';
import useChat from '../composables/useChat';

const newMessage =  ref('')

const { messages, unsubscribe } = useChat();
onUnmounted(() => {
    unsubscribe()
})
</script>

<template>
    <h1 class="text-6xl font-extrabold tracking-tighter text-center mt-8">My Chat</h1>
    <div class="min-h-[500px] w-full mt-8 rounded-lg shadow-2xl flex flex-col justify-between">
        <ul class="p-4 space-y-4">
            <li v-for="message in messages" :key="message.id">
                <div class="flex justify-between bg-gray-200 px-4 py-2 rounded-lg"><span>{{message.text}}</span><span>by {{message.author}}</span></div>
            </li>
        </ul>
        <div>
            <input class="w-full p-4 rounded-lg focus:outline-none focus:bg-blue-100" type="text" placeholder="Type a message..." v-model="newMessage"/>
        </div>
    </div>
</template>