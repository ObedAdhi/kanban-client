<template>

  <section id="mainPage" class="mt-2">
    <div class="container-fluid scrollX"  >
      <div class="d-flex justify-content-start">
        <CategoryContainer 
        v-for="category in allCategory"
        :key="category.id"
        :category="category"

        @getCategoryId="sendCategoryId"
        @handleTaskId="handleTaskId"
        @handleTaskIdForUpdate="handleTaskIdForUpdate"
        @handleTaskData="handleTaskData"
        @handleDeleteCategory="handleDeleteCategory"
        ></CategoryContainer>
      </div>
    </div>  
  </section>

</template>

<script>
import CategoryContainer from "./CategoryContainer"

export default {
  name: "MainPage",
  data() {
    return {

    }
  },
  methods: {
    sendCategoryId(param) {
      console.log(param);
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
  components: {
    CategoryContainer
  },
  props: ["allCategory"]

}
</script>

<style>

</style>