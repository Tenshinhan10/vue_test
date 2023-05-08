<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-form @submit="onSubmit" class="q-gutter-md">
      <q-input outlined v-model="id" label="ID" readonly />
      <q-input outlined v-model="fname" label="Firstname" />
      <q-input outlined v-model="lname" label="Lastname" />
      <q-input outlined v-model="username" label="Username" />
      <q-input outlined v-model="password" label="Password" />
      <q-input outlined v-model="email" label="Email" />
      <q-input outlined v-model="image" label="URL Image" />
      <q-btn label="Update" type="submit" color="primary" />
    </q-form>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import router from "../router";

const route = useRoute();
const id = ref(route.params.id);
const fname = ref("");
const lname = ref("");
const username = ref("");
const password = ref("");
const email = ref("");
const image = ref("");

const fetchData = () => {
  fetch(`http://localhost:5000/data/${id.value}`)
    .then((res) => res.json())
    .then((result) => {
      console.log("User data:", result);
      fname.value = result.fname;
      lname.value = result.lname;
      username.value = result.username;
      password.value = result.password;
      email.value = result.email;
      image.value = result.image;
    })
    .catch((error) => {
      console.log("Error fetching user data:", error);
    });
};
fetchData();


const onSubmit = () => {
  const myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");

  const raw = JSON.stringify({
    fname: fname.value,
    lname: lname.value,
    username: username.value,
    password: password.value,
    email: email.value,
    image: image.value,
  });

  const requestOptions = {
    method: "PUT",
    headers: myHeaders,
    body: raw,
    redirect: "follow",
  };

  fetch(`http://localhost:5000/data/${id.value}`, requestOptions)
    .then((response) => response.json())
    .then((result) => {
      alert("Successful data update");
      router.push("/");
    })
    .catch((error) => console.log("error", error));
};
</script>
