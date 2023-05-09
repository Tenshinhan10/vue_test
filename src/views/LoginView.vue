<template>
  <center>
    <h3>Login Form</h3>
    <div class="q-pa-md" style="max-width: 400px">
      <form class="q-gutter-md" @submit.prevent="submitForm">
        <q-input outlined v-model="username" label="Username" type="text" />
        <q-input outlined v-model="password" label="Password" type="password" />
        <span>Do you have an account? <router-link to="/about">Register</router-link><br></span>
        <q-btn label="Login" type="submit" color="primary" />
      </form>
    </div>
  </center>
</template>

<script>
import { ref } from 'vue';
import router from '../router';

export default {
  setup() {
    const username = ref("");
    const password = ref("");

    const submitForm = () => {
      if (!username.value || !password.value) {
        alert("Please enter both username and password.");
        return;
      }
      const requestBody = { username: username.value, password: password.value };
      fetch("http://localhost:5000/login", {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(requestBody)
      })
      .then(response => {
        if (response.status === 200) {
          alert("Login successfully.");
          router.push('/');
        } else {
          alert("Login failed. Please try again.");
        }
      })
      .catch(error => console.log('error', error));
    };
    return { username, password, submitForm };
  }
}
</script>

