<script setup>
import { inject, ref, nextTick } from "vue";
import { collection, query, onSnapshot, orderBy } from "firebase/firestore";
import { auth, db } from "src/firebase/firebaseConfig";

const userGoogle = inject("userGoogle");
const q = query(collection(db, "chat"), orderBy("time", "asc"));
const chatMessages = ref([]);
const chatContainerRef = ref(null);

const unsubscribe = onSnapshot(q, (snapshot) => {
  // console.log(userGoogle.value);
  // if (!userGoogle.value) return;

  snapshot.docChanges().forEach(async (change) => {
    if (change.type === "added") {
      console.log("nuevo mensaje recibido: ", change.doc.data());
      chatMessages.value.push({
        uid: change.doc.id,
        ...change.doc.data(),
      });

      await nextTick();
      window.scrollTo(0, chatContainerRef.value.scrollHeight);

      // setTimeout(() => {
      //   if (chatContainerRef.value !== null) {
      //     console.log(chatContainerRef.value.scrollHeight);
      //     window.scrollTo(0, chatContainerRef.value.scrollHeight);
      //   }
      // }, 600);
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
    <div class="q-pa-md row justify-center" ref="chatContainerRef">
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
