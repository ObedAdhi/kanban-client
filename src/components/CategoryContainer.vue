<template>

  <div class="d-inline-flex p-3 " style="width: 300px; height: 620px;" id="cardholder1">
    <div class="card " style="width: 300px; background-color: #eeeeee;"> 
      <div class="card-header bg-dark text-light"> {{ category.name }}
        
        <button @click.prevent="handleDeleteCategory(category.id)" class="btn btn-sm btn-rounded floatRight btn-danger">
          <i  class="far fa-trash-alt"></i>
        </button>
      </div>
      <div class="card-body scrollY m-0 p-0">
        <TaskCard 
        v-for="task in category.Tasks" 
        :key="task.id"
        :task="task"
        :allCategory="allCategory"
        @handleTaskId="handleTaskId"
        @handleTaskIdForUpdate="handleTaskIdForUpdate"
        @handleTaskData="handleTaskData"
        ></TaskCard>
      </div>
      <button @click.prevent="sendCategoryId(category.id)" class="btn">add new task</button>
    </div>
  </div>


</template>

<script>
import TaskCard from "./TaskCard"

export default {
  name: "CategoryContainer",
  components: {
    TaskCard
  },
  methods: {
    sendCategoryId(param) {
      // console.log(param);
      this.$emit("getCategoryId", param)
    },
    handleTaskId(param) {
      this.$emit("handleTaskId", param)
    },
    handleTaskIdForUpdate(param) {
      this.$emit("handleTaskIdForUpdate", param)
    },
    handleTaskData(param) {
      this.$emit("handleTaskData", param)
    },
    handleDeleteCategory(param) {
      this.$emit("handleDeleteCategory", param)
    }
  },
  props: ["allCategory","category"]
}
</script>

<style>

</style>