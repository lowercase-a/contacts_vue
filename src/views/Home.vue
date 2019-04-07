<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>firstName: <input type="text" v-model="newContact.first_name"></p>
    <p>lastName: <input type="text" v-model="newContact.last_name"></p>
    <p>email: <input type="text" v-model="newContact.email"></p>
    <p>phoneNumber: <input type="text" v-model="newContact.phone_number"></p>
    <p>middleName: <input type="text" v-model="newContact.middle_name"></p>
    <p>bio: <input type="text" v-model="newContact.bio"></p>
    <button v-on:click="makeContact()">Make the contact</button>
    <div v-for="contact in contacts">
      {{ contact.full_name }}
      {{ contact.email }}
      <button v-on:click="currentContact = contact">Show more info</button>
      <div v-if="currentContact === contact">
        {{ contact.phone_number }}
        {{ contact.bio }}
        <p>firstName: <input type="text" v-model="contact.first_name"></p>
        <p>lastName: <input type="text" v-model="contact.last_name"></p>
        <p>email: <input type="text" v-model="contact.email"></p>
        <p>phoneNumber: <input type="text" v-model="contact.phone_number"></p>
        <p>middleName: <input type="text" v-model="contact.middle_name"></p>
        <p>bio: <input type="text" v-model="contact.bio"></p>
        <button v-on:click="updateContact(contact)">Update contact</button>
        <button v-on:click="destroyContact(contact)">Destroy contact</button>
      </div>
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
        first_name: "",
        last_name: "",
        email: "",
        phone_number: "",
        middle_name: "",
        bio: ""
      },
      currentContact: {}
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
      var params = this.newContact;
      // send the data to the api
      axios.post("/api/contacts", params).then(response => {
        console.log(response);
        // add the data to the array
        this.contacts.push(response.data)
      });
    },
    updateContact: function(contact) {
      axios.patch("/api/contacts/" + contact.id, contact).then(response => {
        console.log(response);
        // find contact in array
        var index = this.contacts.indexOf(contact);
        // replace contact in array
        this.contacts.splice(index, 1, response.data);
      })
    },
    destroyContact: function(contact) {
      axios.delete("/api/contacts/" + contact.id).then(response => {
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      })
    }
  }
};
</script>

