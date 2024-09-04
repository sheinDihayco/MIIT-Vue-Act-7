<template>
  <main>
    <div class="container-bar p-3 m-3 bg-info"> <!--bg-success-->
      <h1 class="text-center">GET ALL RECORDS</h1>
      <button @click="getRecord()" class="btn btn-warning d-inline-block me-2 m-2">Get</button>
      <table class="table table-striped">

        <tbody>
          <tr>
            <th>No.</th>
            <th>First Name</th>
            <th>Last Name</th>
          </tr>
          <tr v-for="(user, i) in users" :key="i">
            <td>{{ i + 1 }}</td>
            <td>{{ user?.fname }}</td>
            <td>{{ user?.lname }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container-bar p-3 m-3">
      <h1 class="text-center">Delete</h1>
      <div>
        <input v-model="id" type="number" class="form-control d-inline-block w-50 me-2">
        <button @click="deleteRecord()" class="btn btn-danger d-inline-block">Delete</button>
      </div>
    </div>
  </main>
</template>
<script lang="ts">

import axios from 'axios';
import Swal from 'sweetalert2';

export default {
  name: "AboutView",
  data() {
    return {
      users: {} as any,
      id: 0
    }
  },
  methods: {
    async getRecord() {
      await axios.get("http://127.0.0.1:8000/api/user/getAll").then(async (response) => {
        this.users = response.data;
      });
    },
    async deleteRecord() {
      Swal.fire({
        title: "Are you sure?",
        text: "Delete selected record?",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      })
        .then(async (willDelete) => {
          if (willDelete) {
            await axios.get("http://127.0.0.1:8000/api/user/deleteByID/" + this.id).then(async (response) => {
              if (response.data?.success) {
                Swal.fire({
                  title: "Deleted",
                  text: response.data?.message,
                  icon: "success",
                  button: "Aww yiss!",
                });
              }
              else {
                Swal.fire({
                  title: "Fail",
                  text: response.data?.message,
                  icon: "warning",
                  button: "Aww yiss!",
                });
              }
            });
          }
        });
    }
  },

}
</script>
<style scope>
.container-bar {
  border: 3px solid black;
}
</style>