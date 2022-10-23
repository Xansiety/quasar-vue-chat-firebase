<template>
  <q-page padding>
    <div class="q-pa-md row justify-center">
      <div style="width: 100%; max-width: 400px">
        <q-chat-message
          name="me"
          :text="['hey, how are you?']"
          stamp="7 minutes ago"
          sent
          bg-color="amber-7"
        />
        <q-chat-message
          name="Xansiety"
          :text="['Fine and u?']"
          stamp="5 minutes ago"
          bg-color="amber-7"
        />
      </div>
    </div>
  </q-page>
</template>
