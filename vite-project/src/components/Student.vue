<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-md-10">
        <h3 class="text-center text-dark mt-2">Laravel 10 Vue JS Vite</h3>
      </div>
    </div>
    <div class="row">
      <div class="col-md-4">
        <div class="card shadow">
          <div class="card-header bg-primary text-white">Add Record</div>
          <div class="card-body">
            <form @submit.prevent="save">
              <div class="form-group">
                <label for="name">Student Name</label>
                <input
                  type="text"
                  v-model="student.name"
                  class="form-control"
                  id="name"
                  placeholder="Enter Student Name"
                />
              </div>
              <div class="form-group">
                <label for="address">Student Address</label>
                <input
                  type="text"
                  v-model="student.address"
                  class="form-control"
                  id="address"
                  placeholder="Enter Student Address"
                />
              </div>
              <div class="form-group">
                <label for="phone">Phone</label>
                <input
                  type="text"
                  v-model="student.phone"
                  class="form-control"
                  id="phone"
                  placeholder="Enter Phone Number"
                />
              </div>
              <div class="mb-3"></div>
              <!-- Add space between input and button -->
              <button type="submit" class="btn btn-primary btn-block">Save</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-8">
        <h2 class="text-center mb-8">Student List</h2>
        <table class="table table-hover">
          <thead class="thead-dark">
            <tr>
              <th scope="col">ID</th>
              <th scope="col">Student Name</th>
              <th scope="col">Address</th>
              <th scope="col">Phone</th>
              <th scope="col">Options</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="student in result" :key="student.id">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.address }}</td>
              <td>{{ student.phone }}</td>
              <td>
                <button
                  type="button"
                  class="btn btn-outline-warning"
                  @click="edit(student)"
                >
                  Edit
                </button>
                &nbsp;
                <button
                  type="button"
                  class="btn btn-outline-danger"
                  @click="remove(student)"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Student",
  data() {
    return {
      result: {},
      student: {
        id: "",
        name: "",
        address: "",
        phone: "",
      },
    };
  },
  created() {
    this.StudentLoad();
  },
  mounted() {
    console.log("mounted() called.......");
  },

  methods: {
    StudentLoad() {
      var page = "http://127.0.0.1:8000/api/student";
      axios.get(page).then(({ data }) => {
        console.log(data);
        this.result = data;
      });
    },

    save() {
      if (this.student.id == "") {
        this.saveData();
      } else {
        this.updateData();
      }
    },
    saveData() {
      axios.post("http://127.0.0.1:8000/api/student", this.student).then(({ data }) => {
        alert("saved");
        this.StudentLoad();
        this.student.name = "";
        (this.student.address = ""), (this.student.phone = "");
        this.id = "";
      });
    },
    edit(student) {
      this.student = student;
    },
    updateData() {
      var editrecords = "http://127.0.0.1:8000/api/student/" + this.student.id;
      axios.put(editrecords, this.student).then(({ data }) => {
        this.student.name = "";
        (this.student.address = ""), (this.student.phone = "");
        this.id = "";
        alert("Updated!!!");
        this.StudentLoad();
      });
    },

    remove(student) {
      var url = `http://127.0.0.1:8000/api/student/${student.id}`;
      axios.delete(url);
      alert("Deleted");
      this.StudentLoad();
    },
  },
};
</script>
