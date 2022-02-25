<template>
  <div id="student-form">
    <form @submit.prevent="onSubmit()">
      <span>Student Id</span>
      <input
        v-model="formData.studentId"
        type="text" />
      <span>Firstname</span>
      <input
        v-model="formData.firstname"
        type="text" />
      <span>Lastname</span>
      <input
        v-model="formData.lastname"
        type="text" />

      <button
        v-if="isEdit"
        type="submit">
        Save
      </button>
      <button
        v-if="isEdit"
        @click="onCancel()">
        Cancel
      </button>
      <button
        v-else
        type="submit">
        Add Student
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'student-form',
  props: {
    isEdit: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      formData: {
        id: null,
        name: '',
        email: ''
      }
    }
  },
  methods: {
    initData (data) {
      this.formData = { ...data }
    },
    onSubmit () {  
      this.$emit('submit:student', this.formData)
    },
    onCancel () {
      this.$emit('cancel:student')
    }
  }
}
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  button {
    margin: 5px 0;
  }
</style>