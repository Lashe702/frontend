<template>
    <div class="p-5 text center">
      <div class="h1">Available Courses</div>
      <div class="pt-5">
        <table class="table">
          <thead class="table-dark">
            <tr>
              <th scope="col">Course</th>
              <th scope="col">Title</th>
              <th scope="col">Credits</th>
              <th scope="col"></th>
            </tr>
          </thead>
          <tbody>
          <tr v-for="(course, i) in courses" :key="i">
          <td>{{ course.name  }}</td>
          <td>{{ course.title }}</td>
          <td>{{ course.credits }}</td>
          <td>
            <button class="mx-2 btn btn-primary" @click="viewCourse(course.name)">View</button>
            <button class="mx-2 btn btn-secondary" @click="editCourse(course._id)">Edit</button> 
            <button class="mx-2 btn btn-danger" @click="deleteCourse(course._id, i)">Delete</button>
          </td>
        </tr>
        <tr>
          <td colspan="3"><router-link class="btn btn-primary" to="/courses/add">Add Course</router-link></td>
        </tr>
        </tbody>
      </table>

      <div>

      </div>

    </div>
  </div>


           </template>
 <script>
import axios from 'axios';

 export default {
  data(){
    return{
      courses: [],                 
     }
  },

  async created() {
    this.courses = (await axios.get('http://localhost:3000/api/courses')).data;
  },
  methods: {
    async deleteCourse(id, i) {
      if(confirm("Do you really want to delete " + this.courses[i].name + "? You won't be able to undo this operation. Press OK to confirm deletion.")){
        axios.post("http://localhost:3000/api/courses/delete", {
          courseID: id,
          
        });
        this.courses.splice(i, 1);
      }

    },
    editCourse(id) {
      this.$router.push({name: 'edit-course', params:{courseID: id}})
    },
    viewCourse(courseName) {
      let coursePath = `/course/${courseName}`
      this.$router.push({path: coursePath})
    }
  }
 }
 </script>
