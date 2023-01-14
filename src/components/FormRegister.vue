<script setup lang="ts">
import { ref } from "vue";
import {
  getAuth,
  createUserWithEmailAndPassword,
  GoogleAuthProvider,
  signInWithPopup,
} from "firebase/auth";
import router from "@/router";
import { RouterLink } from "vue-router";
import Button from "@/components/Button.vue";

const email = ref("");
const password = ref("");
const errorMessage = ref("");

const register = () => {
  createUserWithEmailAndPassword(getAuth(), email.value, password.value)
    .then((data) => {
      router.push("/chat");
    })
    .catch((error) => {
      console.error(error);

      switch (error.code) {
        case "auth/invalid-email":
          errorMessage.value = "Invalid email";
          break;

        case "auth/email-already-in-use":
          errorMessage.value = "There is already an account with this email";
          break;

        default:
          errorMessage.value = "Please fill in the empty fields";
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
    class="flex flex-wrap flex-column gap align-center justify-center card card-main"
  >
    <h1>Create an Account</h1>

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
        @on-click="register"
        type="button primary mr-16"
        content="Sign Up"
      />
      <Button
        @on-click="signInWithGoogle"
        type="button primary"
        content="Sign In With Google"
      />
    </div>

    <RouterLink to="/" class="link-light">Already have an account? </RouterLink>
  </div>
</template>

<style scoped lang="scss">
@use "../assets/scss/components/button";
@use "../assets/scss/base/transition";
</style>
