<template>
    <!-- Child of student table -->
    <tr v-bind:class="{ present: student.present, absent: !student.present }"> 
        <td>{{ student.name }}</td>
        <td>{{ student.starID }}</td>
        <td><input type="checkbox"  v-on:change="arrivedOrLeft(student, $event.srcElement.checked)"></td>
        <td v-show="edit"> <img v-on:click="deleteStudent" src="@/assets/delete.png"> </td>
    </tr>
</template>

<script>
export default {
    name: 'StudentRow',
    props: {
        student: Object,
        edit: Boolean
    },
    methods: {
        arrivedOrLeft(student, present) {
           this.$emit('student-arrived-or-left', student, present)
        },
        deleteStudent() { // When delete icon is clicked it emits the delete student event
            if (confirm(`Delete ${this.student.name}?`)) {
                this.$emit('delete-student', this.student) // Then "carries the payload" of this.students to student table
            }
        }
    }
}
</script>

<style scoped>

.present {
    color: grey;
    font-style:italic;
}
.absent {
    color: black;
    font-weight: bold;
}
img {
    height: 25px;
}

</style>