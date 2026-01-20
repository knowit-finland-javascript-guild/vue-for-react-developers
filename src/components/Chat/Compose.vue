<script setup>
import { ref } from "vue";
import Input from "../generic/form/Input.vue";
import Button from "../generic/form/Button.vue";
import useIcons from "../../compositionFunctions/useIcons";

// Store the text input value. This is a reactive 'state' variable.
const text = ref("");

/**
 * Handle changes to the text input value
 * @param {String} newValue New text value
 */
const onChange = (newValue) => {
  text.value = newValue;
};

// Components emit events. Parent components listen to these events. Just like the DOM!
const emit = defineEmits(["send"]);

const icons = useIcons();

/**
 * Sends a message.
 * @param {string} content - The content of the message.
 * @param {string} type - The type of the message (text or emoji).
 */

function send() {
  // Hint: Call 'emit' with the correct arguments.
  emit("send", { content: text.value, type: "text" });
  // Finally, clear the text input.
  text.value = "";
}

/**
 * Sends an emoji.
 * @param {String} name
 */
function sendEmoji(name) {
  emit("send", { content: name, type: "emoji" });
}
</script>

<template>
  <div class="compose">
    <div class="emoji-selector">
      <!-- Hint: Use the 'sendEmoji' function to send an emoji. -->
      <Button
        v-for="icon in Object.keys(icons)"
        :key="icon"
        :icon="icon"
        :alt="icon"
        @click="sendEmoji(icon)"
      />
    </div>
    <div class="message-row">
      <!-- Hint: See if you could send the message by pressing the enter key in the text field.
                 https://vuejs.org/guide/essentials/event-handling.html#key-modifiers
      -->
      <Input
        :value="text"
        placeholder="Type a message"
        @change="onChange"
        @keyup.enter="send()"
      />

      <!-- Hint: Call the 'send()' function when the button emits a 'click' event. -->
      <Button icon="send" @click="send()" />
    </div>
  </div>
</template>

<style scoped>
.message-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5rem;
}
/* This targets a nested component with scoped styles*/
.compose:deep(.emoji-selector) {
  margin-bottom: 0.5rem;
}
</style>
