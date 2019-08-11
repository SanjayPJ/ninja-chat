<template>
  <v-card max-width="500" class="mx-auto my-12">
    <v-card-text>
      <h1 class="text-center mt-5 pb-10 indigo--text">Welcome</h1>
      <form @submit.prevent="formSubmit">
        <v-flex>
          <v-text-field v-model="name" label="Enter your Name" outlined></v-text-field>
        </v-flex>
        <v-btn color="indigo" type="submit" block dark large>Enter Chat</v-btn>
      </form>
    </v-card-text>
      <v-snackbar top class="mt-3" v-model="snackbar">{{ text }}</v-snackbar>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      name: null,
      snackbar: false,
      text: ""
    };
  },
  methods: {
    formSubmit() {
      if (this.name) {
        this.$router.push({ name: "chat", params: { person: this.name } });
      } else {
        this.text = "Please enter a valid name 😜";
        this.snackbar = true;
      }
    }
  }
};
</script>