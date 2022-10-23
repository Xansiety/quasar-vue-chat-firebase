<script setup>
import { onAuthStateChanged } from "firebase/auth";
import { ref, provide } from "vue";
import { auth } from "./firebase/firebaseConfig";

const userGoogle = ref(false);
provide("userGoogle", userGoogle);
onAuthStateChanged(auth, (user) => {
  // console.log({ user });
  userGoogle.value = user;
});
</script>

<template>
  <router-view />
</template>
