<template>

  <div class="d-inline-flex p-3" style="width: 300px; height: 250px;" id="cardholder4">
    <div class="card p-2" style="width: 500px; background-color: #eeeeee;"> 
      <h3>Add New Task</h3><br>
      <form @submit.prevent="handleTitleForAddTask">
        <div class="form-outline mb-4">
          <input v-model="title" type="text" id="addTaskTitle" class="form-control bg-light" />
          <label class="form-label" for="addTaskTitle">Title</label>
        </div>
        <button type="submit" class="btn btn-primary m-2">Create</button>
        <button class="btn btn-danger right m-2" 
        v-on:click.prevent="switchLoginRegister('main')">Cancel</button>
      </form>
    </div>
  </div>

</template>

<script>
export default {
  name: "TaskAddForm",
  data () {
    return {
      server: "http://localhost:3000",
      title: "",
      categoryId: ""
    }
  },
  methods: {
    handleTitleForAddTask() {
      let param = this.title
      this.$emit("carryTitle", param)
    },
  },
  props: ["switchLoginRegister"],

}
</script>

<style>
.right {
  float: right;
}
</style>