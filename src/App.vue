<script setup>
import { onAuthStateChanged } from "firebase/auth";
import { ref, provide } from "vue";
import { auth } from "./firebase/firebaseConfig";
import { useQuasar } from "quasar";
const userGoogle = ref(false);
const $q = useQuasar();

provide("userGoogle", userGoogle);

$q.loading.show();

onAuthStateChanged(auth, (user) => {
  // console.log({ user });
  userGoogle.value = user;
  $q.loading.hide();
});
</script>

<template>
  <router-view v-if="userGoogle !== false" />
</template>

<style>
body {
  overflow-y: hidden;
}
</style>
