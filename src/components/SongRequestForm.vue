<template>
    <div class="request-container">
      <h2>Request a Song</h2>
      <form @submit.prevent="submitRequest">
        <input
          type="text"
          v-model="songName"
          placeholder="Enter song name"
          required
        />
        <button type="submit">Send Request</button>
      </form>
      <p v-if="successMessage" class="success">{{ successMessage }}</p>
    </div>
  </template>
  
  <script lang="ts">
  import axios from "axios";
  
  export default {
    data() {
      return {
        songName: "",
        successMessage: "",
      };
    },
    methods: {
      async submitRequest() {
        try {
          await axios.post("http://localhost:6000/api/songs", {
            songName: this.songName,
          });
          this.successMessage = "Request sent successfully!";
          this.songName = ""; // Clear the input field
        } catch (error) {
          console.error("Error sending request:", error);
          this.successMessage = "Failed to send request.";
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .request-container {
    text-align: center;
    max-width: 400px;
    margin: auto;
  }
  input {
    width: 100%;
    padding: 8px;
    margin: 10px 0;
  }
  button {
    padding: 8px 15px;
    background-color: #007bff;
    color: white;
    border: none;
    cursor: pointer;
  }
  button:hover {
    background-color: #0056b3;
  }
  .success {
    color: green;
    margin-top: 10px;
  }
  </style>
  