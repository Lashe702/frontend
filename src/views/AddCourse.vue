<template>
    <div class="pt-5">
      <div class="h1 text-center">Add Course</div>
      <div class="pt-2" style="margin-left: auto; margin-right auto; width: 80%;">
  <form>
    <div class="form-group">
      <label for="nameInput">
        Course:
      </label>
      <input type="text" id="nameInput" class="form-control" ref="courseName" />
    <small id="nameHelp" class="form-text text-muted">
      Example: SDEV-255
    </small>
  </div>
    <div class="form-group my-2">
      <label for="titleInput">
        Title:
      </label>
      <input type="text" id="titleInput" class="form-control" ref="courseTitle" />
  </div>
  <button class="btn btn-primary" @click.prevent="getFormValues()">
Confirm
  </button>
  </form>
  <div id="alert" class="mt-5 alert" role="alert" :class="alertType">
  </div>
</div>
</div>
  </template>

  <script>
    import axios from 'axios';

    export default {
      data() {
        return {
          courseName: '',
          courseTitle: '',
          errors:[],
          alertType: {
            active: false,
            "alert-danger": false,
            "alert-success": false,
          },
        };
      },

      methods: {
        getFormValues(){
          this.errors = [];
          this.courseName = this.$refs.courseName.value;
          this.courseTitle = this.$refs.courseTitle.value;
          
          if (this.courseName && this.courseTitle) {
            this.postCourse();
            this.alertType = {
            active: true,
            "alert-danger": false,
            "alert-success": true,
          }
          document.getElementById("alert").innerHTML = '<p style="margin: 0; padding: 0"><strong>Success</strong>: Course was updated successfully. Go backto <a class="alert-link" href="courses">Courses</a>.'
          } else {
            if (!this.courseName) this.errors.push("No course name")
            if (!this.courseTitle) this.errors.push("No Title")
            this.alertType = {
               active: true,
              "alert-danger": true,
              "alert-success": false,
          }
          let alertText="";
          for (let error of this.errors) {
            alertText += `<p style="margin: 0; padding: 0;"><strong>Error</strong>: ${error}</p>`
          }
          document.getElementById("alert").innerHTML = alertText
          }
        },
        async postCourse() {
          await axios.post("http://localhost:3000/api/course/add",{
            name: this.courseName,
            title: this.courseTitle
          })
        }
      }
    };
  </script>
