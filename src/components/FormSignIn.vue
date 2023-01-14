<script setup lang="ts">
import { ref, reactive } from "vue";
import {
  getAuth,
  signInWithEmailAndPassword,
  GoogleAuthProvider,
  signInWithPopup,
} from "firebase/auth";
import router from "@/router";
import { RouterLink } from "vue-router";
import Button from "@/components/Button.vue";
import AccountView from "@/views/AccountView.vue";

const email = ref("");
const password = ref("");
const errorMessage = ref("");

const signIn = () => {
  signInWithEmailAndPassword(getAuth(), email.value, password.value)
    .then((data) => {
      router.push("/chat");
    })
    .catch((error) => {
      console.error(error);

      switch (error.code) {
        case "auth/invalid-email":
          errorMessage.value = "Invalid email";
          break;

        case "auth/wrong-password":
          errorMessage.value = "Incorrect password";
          break;

        case "auth/user-not-found":
          errorMessage.value = "No account with that email was found";
          break;

        default:
          errorMessage.value = "Email or password was incorrect";
          break;
      }
    });
};

const signInWithGoogle = () => {
  const provider = new GoogleAuthProvider();
  signInWithPopup(getAuth(), provider)
    .then((result) => {
      router.push("/chat");
    })
    .catch((error) => {
      console.error(error);
    });
};
</script>

<template>
  <div
    class="flex flex-wrap flex-column align-center justify-center gap card card-main"
  >
    <h1>Sign In</h1>

    <input
      type="text"
      placeholder="E-Mail"
      autocomplete="off"
      v-model="email"
    />
    <input type="password" placeholder="Password" v-model="password" />

    <Transition>
      <p style="color: lightcoral" v-if="errorMessage">{{ errorMessage }}</p>
    </Transition>

    <div class="flex flex-wrap">
      <Button
        @on-click="signIn"
        type="button primary mr-16"
        content="Sign In"
      />
      <Button
        @on-click="signInWithGoogle"
        type="button primary"
        content="Sign In With Google"
      />
    </div>

    <RouterLink to="/register" class="link-light"
      >Don't have an account?
    </RouterLink>
  </div>
</template>

<style scoped lang="scss">
@use "../assets/scss/base/transition";
</style>
