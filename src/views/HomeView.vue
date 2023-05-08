<template>
  <div class="q-pa-md">
    <div class="q-py-md">
      <q-btn icon="add" @click="onCreate()"></q-btn>
    </div>
    <q-table title="Data" :rows="rows" :columns="columns" row-key="name">
      <template v-slot:body-cell-image="props">
        <q-td :props="props">
          <q-img :src="props.row.image"></q-img>
        </q-td>
      </template>
      <template v-slot:body-cell-actions="props">
        <q-td :props="props">
          <q-btn icon="mode_edit" @click="onEdit(props.row.id)"></q-btn>
          <q-btn icon="delete" @click="onDelete(props.row.id)"></q-btn>
        </q-td>
      </template>
    </q-table>
  </div>
</template>

<script setup>
import router from "../router";
import { ref } from "vue";

const columns = ref([
  { name: "id", align: "left", label: "ID", field: "id", sortable: true },
  {
    name: "fname",
    align: "left",
    label: "fname",
    field: "fname",
    sortable: true,
  },
  {
    name: "lname",
    align: "left",
    label: "lname",
    field: "lname",
    sortable: true,
  },
  {
    name: "username",
    align: "left",
    label: "username",
    field: "username",
    sortable: true,
  },
  { name: "image", align: "center", label: "image", field: "image" },
  {
    name: "actions",
    align: "center",
    label: "actions",
    field: "id",
    sortable: true,
  },
]);

const rows = ref([]);
const fetchData = () => {
  fetch("http://localhost:5000/data")
    .then((res) => res.json())
    .then((result) => {
      console.log("User data:", result);
      rows.value = result;
    })
    .catch((error) => {
      console.log("Error fetching user data:", error);
    });
};
fetchData();

const onEdit = (id) => {
  router.push('/update/' + id);
};

const onDelete = (id) => {
  var myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");

  var requestOptions = {
    method: 'DELETE',
    headers: myHeaders,
    body: JSON.stringify({ id }),
    redirect: 'follow'
  };

  fetch(`http://localhost:5000/data/${id}`, requestOptions)
    .then(response => {
      if (response.ok) {
        alert('Successful data deletion');
        fetchData();
        router.push('/');
      }
      throw new Error('Network response was not ok.');
    })
    .then(result => {
      fetchData();
      router.push('/');
    })
    .catch(error => console.log('error', error));
};



const onCreate = () => {
  router.push("/create");
};

</script>
