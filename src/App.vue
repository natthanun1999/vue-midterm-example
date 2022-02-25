<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1> Students </h1>
  <student-form
    ref="student-form"
    :is-edit="isEdit"
    @submit:student="onFormSubmit($event)"
    @cancel:student="clearStudentForm()" />

  <student-table
    :students="students"
    @on-edit="onSelectStudent($event)"
    @on-delete="deleteStudent($event)" />
</template>

<script>
import StudentTable from './components/StudentTable.vue'
import StudentForm from './components/StudentForm.vue'

export default {
  name: 'App',
  components: {
    StudentTable,
    StudentForm
  },
  data () {
    return {
      students: [],
      isEdit: false,
      selectedStudent: {
        id: null,
        studentId: '',
        firstname: '',
        lastname: ''
      }
    }
  },
  mounted () {
    this.getStudents()
  },
  methods: {
    async getStudents () {
      try {
        let response = await fetch('http://localhost:3000/students')
        response = await response.json()
        this.students = response
      } catch (error) {
        console.error('getStudents', error)
        alert('Error!')
      }
    },
    onSelectStudent (student) {
      this.selectedStudent = { ...student }
      this.isEdit = true
      this.$refs['student-form'].initData(this.selectedStudent)
    },
    async addStudent () {
      try {
        await fetch('http://localhost:3000/students', {
          method: 'POST',
          body: JSON.stringify({ ...this.selectedStudent }),
          headers: {
            'Content-type': 'application/json; charset=UTF-8'
          }
        })
      } catch (error) {
        console.error('addStudent', error)
        alert('Error!')
      }
    },
    async editStudent () {
      try {
        await fetch(`http://localhost:3000/students/${this.selectedStudent.id}`, {
          method: 'PUT',
          body: JSON.stringify({ ...this.selectedStudent }),
          headers: {
            'Content-type': 'application/json; charset=UTF-8'
          }
        })
      } catch (error) {
        console.error('editStudent', error)
        alert('Error!')
      }
    },
    clearStudentForm () {
      this.selectedStudent = {
        id: null,
        studentId: '',
        firstname: '',
        lastname: ''
      }
      this.isEdit = false
      this.$refs['student-form'].initData(this.selectedStudent)
    },
    async deleteStudent (student) {
      try {
        await fetch(`http://localhost:3000/students/${student.id}`, {
          method: 'DELETE'
        })
        this.getStudents()
      } catch (error) {
        console.error('deleteStudent', error)
        alert('Error!')
      }
    },
    async onFormSubmit (student) {
      this.selectedStudent = { ...student }

      if (this.isEdit) {
        await this.editStudent()
      } else {
        await this.addStudent()
      }
      this.clearStudentForm()
      this.getStudents()
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
#helloText {
  color: #178bff;
}
</style>
