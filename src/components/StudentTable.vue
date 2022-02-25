<template>
<div id="helloText" align="center">
  <table v-if="students.length">
    <thead>
      <tr>
        <th>Student ID</th>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="student in students"
        :key="student.id">
        <td>
          {{ student.studentId }}
        </td>
        <td>
          {{ student.firstname }}
        </td>
        <td>
          {{ student.lastname }}
        </td>
        <td>
          <button @click="onEdit(student)">
            Edit
          </button>
          <button @click="onDelete(student)">
            Delete
          </button>
        </td>
      </tr>
    </tbody>
  </table>
  <span v-else>
    No data
  </span>
</div>


</template>

<script>
export default {
  name: 'MyComponent',
  props: {
    students: Array,
  },
  methods: {
    onEdit (student) {
      this.$emit('on-edit', student)
    },
    onDelete (student) {
      this.$emit('on-delete', student)
    },
  }
}
</script>


<style>
  #helloText {
    color: #ff8b00;
  }
</style>