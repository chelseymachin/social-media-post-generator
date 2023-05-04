<script setup lang="ts">
import { Message, User } from '~~/types';


const props = defineProps<{
  messages: Message[]
  users: User[]
  me: User
  usersTyping?: User[]
}>()

function getUser(userId: string) {
  return props.users.find(user => user.id === userId)
}

const isOpen = ref(false)


</script>

<template>
  <div class="fixed bottom-[10px] right-[10px]">
    <button 
      v-show="!isOpen" 
      class="bg-purple-300 p-3 rounded" 
      @click="isOpen = true"
    >
      <IconChat class="w-8 h-8 text-white" />
    </button>

    <div 
      v-if="isOpen" 
      class="box bg-purple-300 dark:bg-purple-700 w-[400px] rounded"
    >
      <header class="bg-purple-200 dark:bg-purple-600 px-4 flex justify-between items-center">
        Support Chat
        <button 
          class="p-4 pr-0" 
          @click="isOpen = false"
        >
          X
        </button>
      </header>


      <div class="messages p-4 overflow-y-scroll max-h-[80vh]" ref="ChatBubble">
        <ChatBubble
          v-for="message in messages"
          :key="message.id"
          :user="getUser(message.userId)"
          :message="message"
        />
      </div>


      <footer class="p-2">
        <input 
          type="text" 
          class="input w-full block" 
          placeholder="Type your support request here"
        />
      </footer>
    </div>
  </div>
</template>
