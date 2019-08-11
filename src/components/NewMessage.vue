<template>
  <v-text-field
    v-on:keyup.enter="messageSubmit"
    v-model="message"
    outlined
    label="Type Here"
    append-outer-icon="send"
    class="pt-6"
  ></v-text-field>
</template>

<script>
import db from "@/firebase/init";

export default {
  name: "NewMessage",
  props: ["person"],
  data() {
    return {
      message: ""
    };
  },
  methods: {
    messageSubmit() {
      if (this.message) {
        db.collection("messages").add({
          content: this.message,
          name: this.person,
          timestamp: Date.now()
        });
        this.message = '';
      }
    }
  }
};
</script>