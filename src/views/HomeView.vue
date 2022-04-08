<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      createNew: {},
      currentContact: {},
    };
  },
  created: function () {
    this.contactIndex();
  },
  methods: {
    contactIndex() {
      axios.get("/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
      });
    },
    createContact() {
      axios.post("/contacts", this.createNew).then((response) => {
        console.log(response.data);
        this.contacts.push(response.data);
      });
    },
    showContact(contact) {
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal(); // streamlined version
      console.log(contact);
    },
    updateContact() {
      axios.patch(`/contacts/${this.currentContact.id}`, this.currentContact).then((response) => {
        console.log("success", response);
        var index = this.contacts.indexOf(this.currentContact);
        this.contacts.splice(index, 1);
        this.contacts.push(response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ contacts.length }}</p>
    <h2>New Contact</h2>
    First
    <input type="text" v-model="createNew.first_name" />
    Last
    <input type="text" v-model="createNew.last_name" />
    Email
    <input type="text" v-model="createNew.email" />
    Image
    <input type="text" v-model="createNew.image" />
    <button v-on:click="createContact()">Create</button>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h3>{{ contact.first_name }} {{ contact.last_name }}</h3>
      <h4>{{ contact.email }}</h4>
      <p>{{ contact.phone_number }}</p>
      <img v-bind:src="contact.image" v-bind:alt="contact.name" style="max-width: 250px" />
      <br />
      <div>
        <button v-on:click="showContact(contact)">More Info</button>
      </div>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <p>First Name: {{ currentContact.first_name }}</p>
        <p>Last Name: {{ currentContact.last_name }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <img v-bind:src="currentContact.image" v-bind:alt="currentContact.first_name" style="max-width: 250px" />
        <p>
          First:
          <input v-model="currentContact.first_name" type="text" />
        </p>
        <p>
          Last:
          <input v-model="currentContact.last_name" type="text" />
        </p>
        <p>
          Email:
          <input v-model="currentContact.email" type="text" />
        </p>
        <p>
          Image:
          <input v-model="currentContact.image" type="text" />
        </p>
        <button v-on:click="updateContact()">Update</button>
        <button>CLOSE</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
