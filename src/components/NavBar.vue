<script setup>
import { ref, inject } from "vue";
import { GoogleAuthProvider, signInWithPopup, signOut } from "firebase/auth";
import EssentialLink from "components/EssentialLink.vue";
import { auth } from "src/firebase/firebaseConfig";

const userGoogle = inject("userGoogle");

const essentialLinks = [
  {
    title: "Docs",
    caption: "quasar.dev",
    icon: "school",
    link: "https://quasar.dev",
  },
];
const rightDrawerOpen = ref(false);
const toggleRightDrawer = () => {
  rightDrawerOpen.value = !rightDrawerOpen.value;
};

const accessGoogle = async () => {
  console.log("Iniciando sesion");
  const provider = new GoogleAuthProvider();
  try {
    const result = await signInWithPopup(auth, provider);
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const user = result.user;
    console.log({ token });
  } catch (error) {
    console.error(error);
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // The email of the user's account used.
    const email = error.customData.email;
    // The AuthCredential type that was used.
    const credential = GoogleAuthProvider.credentialFromError(error);
  }
};

const logoutGoogle = async () => {
  console.log("Cerrando sesion");
  try {
    await signOut(auth);
  } catch (error) {
    console.error(error);
  }
};
</script>

<template>
  <q-header bordered class="bg-primary text-white">
    <q-toolbar>
      <q-toolbar-title>
        <q-avatar>
          <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
        </q-avatar>
        Qusar chat
      </q-toolbar-title>

      <q-btn
        color="secondary"
        icon="mdi-google"
        label="Ingresar"
        @click="accessGoogle"
        v-if="!userGoogle"
      />
      <q-btn
        color="accent"
        icon="mdi-logout-variant"
        label="Salir"
        @click="logoutGoogle"
        v-if="userGoogle"
      />
      <q-btn
        dense
        flat
        round
        icon="menu"
        @click="toggleRightDrawer"
        v-if="userGoogle"
      />
    </q-toolbar>
  </q-header>

  <q-drawer
    v-if="userGoogle"
    show-if-above
    v-model="rightDrawerOpen"
    side="right"
    bordered
  >
    <q-list>
      <q-item-label header> Essential Links </q-item-label>

      <EssentialLink
        v-for="link in essentialLinks"
        :key="link.title"
        v-bind="link"
      />
    </q-list>
  </q-drawer>
</template>
<style lang="scss" scoped></style>
