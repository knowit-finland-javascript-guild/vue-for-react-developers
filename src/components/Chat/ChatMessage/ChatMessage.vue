<script setup>
// A message in the chat.
import { inject } from "vue";
import ChatBubble from "./ChatBubble.vue";
import Timestamp from "../../Timestamp.vue";
import useIcons from "../../../compositionFunctions/useIcons";

const props = defineProps({
  message: {
    type: Object,
    default: () => ({
      type: null,
      userId: null,
      direction: null,
      timestamp: null,
    }),
  },
});

// Inject the provided userId value into this component
const userId = inject("userId");

// Which direction is the bubble originating from?
const direction = props.message.senderId === userId ? "right" : "left";
const icons = useIcons();
</script>

<template>
  <ChatBubble v-if="props.message.type === 'text'" :direction="direction">
    {{ message.content }}
    <Timestamp :date="props.message.timestamp" />
  </ChatBubble>

  <!-- If the message type is not 'text', render something else. Perhaps an emoji?
       Hint: See the README's resources section for a link to Vue's directives
             There's something for conditional rendering there!
  -->
  <ChatBubble v-else-if="props.message.type === 'emoji'" :direction="direction">
    <img :src="icons[message.content]" :alt="message.content" />
    <Timestamp :date="props.message.timestamp" />
  </ChatBubble>
</template>

<style scoped>
.timestamp {
  display: block;
  text-align: right;
}
</style>
