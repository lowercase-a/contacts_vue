<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts">
      {{ contact.full_name }}
      {{ contact.email }}
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
  import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      contacts: []
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      console.log(response);
      this.contacts = response.data;
    });
  },
  methods: {}
};
</script>

