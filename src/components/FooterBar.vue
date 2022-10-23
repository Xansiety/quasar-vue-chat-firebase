<script setup>
import { ref } from "vue";
import { collection, addDoc } from "firebase/firestore";
import { auth, db } from "src/firebase/firebaseConfig";

const message = ref("");

const sendMessage = () => {
  addDoc(collection(db, "chat"), {
    message: message.value,
    uid: auth.currentUser.uid, //no trae el usuario activo y viene por defecto
    time: Date.now(),
    displayName: auth.currentUser.displayName,
  })
    .then(() => {
      console.log("Reproducir sonido");
      message.value = "";
    })
    .catch((error) => console.error(error));
};
</script>

<template>
  <q-footer reveal bordered class="bg-grey-8 text-white">
    <q-toolbar>
      <q-input
        dark
        dense
        standout
        v-model="message"
        class="q-ml-md full-width"
        @keyup.enter="sendMessage"
      >
        <template v-slot:append>
          <q-icon
            name="send"
            type="submit"
            class="cursor-pointer"
            @click="sendMessage"
          />
        </template>
      </q-input>
    </q-toolbar>
  </q-footer>
</template>
