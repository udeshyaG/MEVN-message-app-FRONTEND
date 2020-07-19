<template>
  <div id="home">
    <!-- Button to display form -->
    <button
      class="btn btn-success form-show-button"
      v-on:click="showForm = !showForm"
    >
      {{ showForm ? "Hide" : "Show" }} Message Form
    </button>

    <!-- Input Form -->
    <form v-show="showForm" v-on:submit.prevent="addMessage" class="form-input">
      <div class="form-group">
        <label for="username">Username</label>
        <input
          type="text"
          class="form-control"
          id="username"
          v-model="newMessage.username"
        />
      </div>

      <div class="form-group">
        <label for="subject">Subject</label>
        <input
          type="text"
          class="form-control"
          id="subject"
          v-model="newMessage.subject"
          required
        />
      </div>

      <div class="form-group">
        <label for="message">Your Message</label>
        <textarea
          class="form-control"
          id="message"
          rows="3"
          v-model="newMessage.message"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="imageURL">Image URL</label>
        <input
          type="text"
          class="form-control"
          id="imageURL"
          v-model="newMessage.imageURL"
        />
      </div>

      <input type="submit" value="Add Message" class="btn btn-primary" />
    </form>

    <!-- Messages -->
    <div
      class="list-unstyled"
      v-for="message in reverseMessages"
      v-bind:key="message._id"
    >
      <li class="media">
        <img
          v-if="message.imageURL"
          v-bind:src="message.imageURL"
          class="mr-3"
          :alt="message.subject"
        />
        <div class="media-body">
          <h4 class="mt-0 mb-1">{{ message.username }}</h4>
          <h5 class="mt-0 mb-1">{{ message.subject }}</h5>
          <p>{{ message.message }}</p>
          <small>Created At: {{ message.created }}</small>
        </div>
      </li>
      <br />
      <hr />
    </div>
  </div>
</template>

<script>
import axios from "axios";

const API_URL = "https://udeshya-mevn-message-app.herokuapp.com/messages";
export default {
  data() {
    return {
      showForm: false,
      messages: [],
      newMessage: {
        username: "",
        subject: "",
        message: "",
        imageURL: "",
      },
    };
  },

  mounted() {
    axios.get(API_URL).then((response) => {
      this.messages = response.data;
    });
  },

  methods: {
    addMessage() {
      console.log(this.newMessage);

      axios
        .post(API_URL, this.newMessage)
        .then((response) => {
          this.messages.push(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  computed: {
    reverseMessages() {
      return this.messages.slice().reverse();
    },
  },
};
</script>

<style scoped>
hr {
  color: white;
}

img {
  max-width: 150px;
}

.form-input {
  margin-bottom: 25px;
}

.form-show-button {
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
