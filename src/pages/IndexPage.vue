<script setup>
import { inject, ref } from "vue";
import { collection, query, onSnapshot, orderBy } from "firebase/firestore";
import { auth, db } from "src/firebase/firebaseConfig";

const userGoogle = inject("userGoogle");
const q = query(collection(db, "chat"), orderBy("time", "asc"));
const chatMessages = ref([]);

const unsubscribe = onSnapshot(q, (snapshot) => {
  snapshot.docChanges().forEach((change) => {
    if (change.type === "added") {
      console.log("nuevo mensaje recibido: ", change.doc.data());
      chatMessages.value.push({
        uid: change.doc.id,
        ...change.doc.data(),
      });
    }
    // if (change.type === "modified") {
    //     console.log("Modified city: ", change.doc.data());
    // }
    // if (change.type === "removed") {
    //     console.log("Removed city: ", change.doc.data());
    // }
  });
});
</script>
<template>
  <q-page padding v-if="!userGoogle">
    <h3 class="text-center text-primary">Debes de iniciar sesión</h3>
  </q-page>
  <q-page v-else padding>
    <div class="q-pa-md row justify-center">
      <div style="width: 100%; max-width: 400px">
        <template v-for="message in chatMessages" :key="message.id">
          <q-chat-message
            :name="message.displayName"
            :text="[message.message]"
            stamp="7 minutes ago"
            :sent="message.uid === auth.currentUser.uid"
            bg-color="amber-7"
          />
        </template>
      </div>
    </div>
  </q-page>
</template>
