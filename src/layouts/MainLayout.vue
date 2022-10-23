<template>
  <q-layout view="hHr lpR fFr">
    <NavBar />

    <q-page-container>
      <!-- {{ userGoogle }} -->
      <router-view />
    </q-page-container>

    <FooterBar v-if="userGoogle" />
  </q-layout>
</template>

<script setup>
import NavBar from "src/components/NavBar.vue";
import FooterBar from "src/components/FooterBar.vue";
import { inject } from "vue";
const userGoogle = inject("userGoogle");
</script>
