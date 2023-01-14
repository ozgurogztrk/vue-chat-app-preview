<script setup lang="ts">
import { getAuth } from "@firebase/auth";
import { ref, reactive, onMounted, onUpdated } from "vue";
import {
  getDatabase,
  ref as storageRef,
  child,
  push,
  update,
  onValue,
} from "@firebase/database";

const db = getDatabase();
const date = reactive({ value: new Date().toLocaleString() });

let message = ref("");
let userData: any = reactive({
  messages: [],
});

const sendMessage = () => {
  const messageData = {
    user: getAuth().currentUser?.email,
    content: message,
    dateTime: date.value,
  };

  const messageKey = push(child(storageRef(db), "messages")).key;

  const updates: any = {};
  updates["/messages/" + messageKey] = messageData;

  return update(storageRef(db), updates)
    .then((response) => {
      messageData.content.value = "" as any;
    })
    .catch((error) => {
      console.error(error);
    });
};

onMounted(() => {
  const allMessages = storageRef(db, "messages/");

  onValue(allMessages, (snapshot) => {
    const data = snapshot.val();
    let messages: any = [];

    Object.keys(data).forEach((key) => {
      messages.push({
        user: data[key].user,
        content: data[key].content._value,
        dateTime: data[key].dateTime,
      });
    });

    userData.messages = messages;
  });
});

onUpdated(() => {
  const chatBox = document.querySelector("#chat-box");
  chatBox?.scrollTo(0, chatBox?.scrollHeight);
});
</script>

<template>
  <div class="flex flex-column justify-end card card-main">
    <div class="flex flex-column overflow-auto" id="chat-box">
      <div
        v-for="message in userData.messages"
        class="flex flex-wrap flex-row w-100 word-break-all"
        :class="
          getAuth().currentUser?.email == message.user
            ? 'justify-flex-end'
            : 'justify-start'
        "
      >
        <p class="h-auto">
          <span class="username">
            <span>
              {{ message.user.substring(0, message.user.indexOf("@")) }}
            </span>

            <span class="date">
              {{ message.dateTime }}
            </span>
          </span>
          {{ message.content }}
        </p>
      </div>
    </div>

    <form
      @submit.prevent="sendMessage"
      class="flex flex-wrap flex-row justify-center gap-column-sm mt-16 w-100"
    >
      <input
        type="text"
        placeholder="Enter message..."
        style="width: 81%"
        v-model="message"
        required
      />
      <button class="button primary">
        <font-awesome-icon icon="fa fa-arrow-right" />
      </button>
    </form>
  </div>
</template>

<style scoped lang="scss">
@use "../assets/scss/components/chat";
@use "../assets/scss/components/button";

@media screen and (max-width: 480px) {
  input {
    width: 65% !important;
  }

  p {
    width: 240px !important;
  }
}

@media only screen and (max-width: 320px) {
  p {
    width: 180px !important;
  }
}
</style>
