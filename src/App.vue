<template>
  <div id="app">
    
    <!-- Prepare App.vue to recieve the message from NewStudentForm with v-on:student-added-->
    <!-- When the event happens we call a method in App.vue newStudentAdded -->
    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <!-- Basically student-arrived-or-left event is being connected to studentArriveOrLeft method -->
    <!-- Listens for event from student table  -->
    <student-table v-bind:students="students" v-on:student-arrived-or-left="studentArrivedOrLeft"
    v-on:delete-student="deleteStudent"></student-table>
    <student-message v-bind:student="mostRecentStudent"></student-message>
    
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) { // Is called when message is emmitted from student=added from NewStudentForm.vue
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() > s2.name.toLowerCase() ? - 1:1
      })
    },
    studentArrivedOrLeft(student, present) {
      // Find student in this.student, set present value
      let updateStudent = this.students.find( function(s) { // function that takes a function as an arguement, will return the first match that this function returns true for 
        if (s.name === student.name && s.starID === student.starID) {
          return true // this will only return true for the students whos name in the array is the same as the student who's being updated name and their starID is the same as the student who's being updated starID 
        }
      })

      if (updateStudent) { // App.vue is making changes to its data and that data is bound to student table's, student prop and the data is updating n student table as well
        updateStudent.present = present
        this.mostRecentStudent = student
      }
    },
    deleteStudent(student) {
      // Filter returns a new array of all students for whom the function return true
      this.students = this.students.filter( function(s) {
        if (s != student) {
          return true
        }
      })

      // TODO clear welcome/goodbye
      this.mostRecentStudent = {}
    }
  }
}
</script>

<style>

</style>
