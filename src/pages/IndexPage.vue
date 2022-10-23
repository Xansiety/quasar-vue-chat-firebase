<script setup>
import { inject, ref, nextTick, watchEffect } from "vue";
import {
  collection,
  query,
  onSnapshot,
  orderBy,
  limit,
} from "firebase/firestore";
import { auth, db } from "src/firebase/firebaseConfig";

const userGoogle = inject("userGoogle");
const chatMessages = ref([]);
const chatContainerRef = ref(null);

let inicio = 0;

//https://vuejs.org/api/reactivity-core.html#watcheffect
watchEffect((onCleanup) => {
  if (userGoogle.value) {
    const q = query(collection(db, "chat"), orderBy("time", "desc"), limit(10));
    const unsubscribe = onSnapshot(q, async (snapshot) => {
      // https://firebase.google.com/docs/firestore/query-data/listen?authuser=1#view_changes_between_snapshots
      snapshot.docChanges().forEach((change) => {
        if (change.type === "added") {
          console.log(change.doc.id);
          // console.log("nuevo mensaje recibido: ", change.doc.data());
          chatMessages.value.push({
            uid: change.doc.id,
            ...change.doc.data(),
          });
        }
      });

      if (inicio === 0) {
        chatMessages.value = chatMessages.value.reverse();
        inicio = 1;
      }

      await nextTick();
      // empuja al final del chat
      chatContainerRef.value.scrollTo(0, chatContainerRef.value.scrollHeight);
    });
    onCleanup(unsubscribe);
  }
});
</script>
<template>
  <q-page padding v-if="!userGoogle">
    <h3 class="text-center text-primary">Debes de iniciar sesión</h3>
  </q-page>
  <q-page v-else padding>
    <div class="q-pa-md row justify-center scrollChat" ref="chatContainerRef">
      <div style="width: 100%; max-width: 400px">
        <template v-for="message in chatMessages" :key="message.id">
          <q-chat-message
            :name="message.displayName"
            :text="[message.message]"
            stamp="7 minutes ago"
            :sent="message.uid === auth.currentUser.uid"
            :bg-color="
              message.uid === auth.currentUser.uid ? 'teal-6' : 'blue-2'
            "
          />
        </template>
      </div>
    </div>
  </q-page>
</template>

<style>
.scrollChat {
  height: 85vh;
  overflow-y: scroll;
}
</style>
