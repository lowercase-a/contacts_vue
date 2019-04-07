<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>firstName: <input type="text" v-model="newContact.firstName"></p>
    <p>lastName: <input type="text" v-model="newContact.lastName"></p>
    <p>email: <input type="text" v-model="newContact.email"></p>
    <p>phoneNumber: <input type="text" v-model="newContact.phoneNumber"></p>
    <p>middleName: <input type="text" v-model="newContact.middleName"></p>
    <p>bio: <input type="text" v-model="newContact.bio"></p>
    <button v-on:click="makeContact()">Make the contact</button>
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
      contacts: [],
      newContact: {
        firstName: "",
        lastName: "",
        email: "",
        phoneNumber: "",
        middleName: "",
        bio: ""
      }
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      console.log(response);
      this.contacts = response.data;
    });
  },
  methods: {
    makeContact: function() {
      console.log('making the contact...');
      // collect all the data
      var params = {
        first_name: this.newContact.firstName,
        last_name: this.newContact.lastName,
        email: this.newContact.email,
        phone_number: this.newContact.phoneNumber,
        bio: this.newContact.bio,
        middle_name: this.newContact.middleName
      }
      // send the data to the api
      axios.post("/api/contacts", params).then(response => {
        console.log(response);
        // add the data to the array
        this.contacts.push(response.data)
      });
    }
  }
};
</script>

