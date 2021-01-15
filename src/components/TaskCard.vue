<template>

  <div class="card mx-3 mt-2">
    <div class="card-body row">
      <div class="hoverme ">
        <h6 class="card-title">{{ task.title }}</h6>
        <p class="card-text smallFont" id="creator">By: {{ task.User.email }}</p>
        <p class="card-text smallFont" id="date">{{ task.createdAt.split("T")[0]  }}</p>
      </div>
      <div class="floatRight" id="hideIt">
        <button @click.prevent="handleTaskData(task)" class="btn btn-sm btn-rounded">
          <i class="fas fa-arrow-alt-circle-right fa-lg"></i>
        </button>
    
        <button @click.prevent="handleTaskIdForUpdate(task.id)" class="btn btn-sm btn-rounded" 
        type="submit"> 
          <i class="far fa-edit"></i> 
        </button>

        <button @click.prevent="handleTaskId(task.id)" class="btn btn-danger btn-sm btn-rounded" 
        type="submit"> 
          <i class="far fa-trash-alt"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskCard",
  props: ["task"],
  methods: {
    handleTaskId(param) {
      this.$emit("handleTaskId", param)
    },
    handleTaskIdForUpdate(param) {
      this.$emit("handleTaskIdForUpdate", param)
    },
    handleTaskData(param) {
      this.$emit("handleTaskData", param)
    }
  },

};
</script>

<style>
</style>