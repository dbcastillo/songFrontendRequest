<template>
  <div class="page-wrapper">
    <div class="request-container">
      <h2>Request a Song</h2>
      <form @submit.prevent="submitRequest">
        <input type="text" v-model="songName" placeholder="Enter song name" required />
        <button type="submit">Send Request</button>
      </form>
      <p v-if="successMessage" class="success">{{ successMessage }}</p>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      songName: '',
      successMessage: '',
    }
  },
  methods: {
    submitRequest() {
      fetch('http://localhost:6001/api/songs', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          songName: this.songName,
        }),
      })
        .then((response) => {
          if (!response.ok) {
            throw new Error('Failed to send request')
          }
          return response.json() // Process response if needed
        })
        .then(() => {
          this.successMessage = 'Request sent successfully!'
          this.songName = '' // Clear the input field
        })
        .catch((error) => {
          console.error('Error sending request:', error)
          this.successMessage = 'Failed to send request.'
        })
    },
  },
}
</script>

<style scoped>
.page-wrapper {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.request-container {
  text-align: center;
  max-width: 400px;
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
