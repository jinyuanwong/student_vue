<template>
  <div id="app">

    <form @submit.prevent="submitForm">
      <div class="form-group row">
        <input type="text" class="form-control col mx-2" placeholder="Name" v-model="student.name">
        <input type="text" class="form-control col mx-2" placeholder="Course" v-model="student.course">
        <input type="text" class="form-control col mx-2" placeholder="Rating" v-model="student.rating">
        <button class="col mx-2 btn btn-success">Submit</button>
      </div>
    </form>

    <table class="table">
      <thead>
        <th>Name</th>
        <th>Course</th>
        <th>Rating</th>
      </thead>   
      <tbody>
        <tr v-for="student in students" :key="student.id" @dblclick="$data.student = student">
          <td>{{ student.name }}</td>
          <td>{{ student.course }}</td>
          <td>{{ student.rating }}</td>
          <td><button class="btn btn-outline-danger btn-sm mx-1" @click="deleteStudent(student.id)">x</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// import {ref} from 'vue'

// const msg = ref('Welcome!');
// const students = ref([]);

// var response = await fetch('http://127.0.0.1:8000/api/students/');

// students.value = await response.json();

// console.log(students.value)

export default {
  name: 'App',
  data(){
    return {

      student: {
        'name': '',
        'course': '',
        'rating': '',
      },

      students: []
    }
  },

  async created(){
    await this.getStudents();
  },

  methods: {
    async getStudents(){
      var response = await fetch('http://127.0.0.1:8000/api/students/');
      this.students = await response.json();
    },

    async createStudent(){
      await fetch('http://127.0.0.1:8000/api/students/', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });

      await this.getStudents();
    },

    async updateStudent(){
      const url = `http://127.0.0.1:8000/api/students/${this.student.id}/`;
      await fetch(url, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });

      await this.getStudents();
    },

    async submitForm(){
      if (this.student.id){
        await this.updateStudent();
      }
      else{
        await this.createStudent();
      }
    },

    async deleteStudent(id) {
      const url = `http://127.0.0.1:8000/api/students/${id}/`;
      await fetch(url, {
        method: 'delete',
      });
      await this.getStudents();
    }
    
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
