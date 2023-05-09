<template>
  <center>
    <h3>Create Form</h3>
    <div class="q-pa-md" style="max-width: 400px">
      <q-form @submit="onSubmit" class="q-gutter-md">
        <q-input outlined v-model="fname" label="Firstname" />
        <q-input outlined v-model="lname" label="Lastname" />
        <q-input outlined v-model="username" label="Username" />
        <q-input outlined v-model="password" label="Password" />
        <q-input outlined v-model="email" label="Email" />
        <q-input outlined v-model="image" label="URL Image" />
        <q-btn label="Submit" type="submit" color="primary" />
      </q-form>
    </div>
  </center>
</template>

<script setup>
import { ref } from 'vue';
import router from '../router';

const fname = ref("");
const lname = ref("");
const username = ref("");
const password = ref("");
const email = ref("");
const image = ref("");

const onSubmit = () =>{
    var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "fname": fname.value,
  "lname": lname.value,
  "username": username.value,
  "password": password.value,
  "email": email.value,
  "image": image.value
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("http://localhost:5000/data", requestOptions)
  .then(response => response.json())
  .then(result => {
    router.push('/')
  })
  .catch(error => console.log('error', error));
}
</script>
