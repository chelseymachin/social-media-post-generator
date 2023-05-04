<script setup lang="ts">
// @ts-expect-error I couldn't find any types for vue 3 markdownit
import Markdown from "vue3-markdown-it";
import { Message, User } from '~~/types';

const props = defineProps<{
    user?: User;
    message?: Message;
    myMessage?: boolean;
}>();
</script>

<template>
    <div 
        class="chat"
        :class="{
            'chat-end': myMessage,
            'chat-start': !myMessage,
        }"    
    >
        <div class="chat-image avatar">
            <div class="rounded-full w-10">
                <img 
                    :src="user?.avatar" 
                    alt="user image" 
                />
            </div>
        </div>
        <div class="chat-header mb-2">
            {{ user?.name }}
            <time v-if="message" class="text-xs opacity-50">
                {{ useTimeAgo(message?.createdAt).value }}
            </time>
        </div>
        <div 
            class="chat-bubble py-0 prose prose-sm bg-pink-400 dark:bg-pink-800 max-w-max w-full"
            :class="{
                'dark:bg-pink-600 bg-pink-500 dark:text-inherit text-white': myMessage,
            }"
        >
            <slot>
                <Markdown :source="message?.text" class="w-full" />
            </slot>
        </div>
        <!-- <div class="chat-footer opacity-50">Delivered</div> -->
    </div>
</template>
<style scoped>
:deep(code) {
    background: none;
    @apply overflow-x-auto w-full;
}
</style>