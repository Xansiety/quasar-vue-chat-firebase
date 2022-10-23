<template>
  <q-layout view="hHr lpR fFr">
    <NavBar />

    <q-page-container>
      <router-view />
    </q-page-container>

    <FooterBar />
  </q-layout>
</template>

<script setup>
import NavBar from "src/components/NavBar.vue";
import FooterBar from "src/components/FooterBar.vue";
</script>
