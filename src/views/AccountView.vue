<script setup lang="ts">
import Button from "@/components/Button.vue";
import router from "@/router";
import { onMounted, ref, reactive } from "vue";
import {
  getAuth,
  onAuthStateChanged,
  signOut,
  type Auth,
} from "@firebase/auth";

const isLoggedIn = ref(false);
const email = reactive({
  value: getAuth().currentUser?.email as String,
});
const userName = ref(email.value.substring(0, email.value.indexOf("@")));
let auth: Auth;

onMounted(() => {
  auth = getAuth();

  onAuthStateChanged(auth, (user) => {
    if (user) {
      isLoggedIn.value = true;
    } else {
      isLoggedIn.value = false;
    }
  });
});

const handleSignOut = () => {
  signOut(auth).then(() => {
    router.push("/");
  });
};
</script>

<template>
  <div
    class="flex flex-wrap flex-column align-center justify-center gap card card-main"
  >
    <h1 v-if="isLoggedIn">{{ userName }}</h1>
    <Button
      v-if="isLoggedIn"
      @on-click="handleSignOut"
      type="button primary"
      content="Sign Out"
    />
  </div>
</template>

<style></style>
