<template>
<div class="container m-0" >
  <div class="row p-3" style="width: 300px; height: 250px;" id="cardholder4">
    <div class="col-md-3" > 
    </div>
    <div class="card p-2 col-md-6" style="width: 300px; background-color: #eeeeee;"> 
      <h3>Add New Category</h3><br>
      <form @submit.prevent="handleNameForCreateCategory">
        <div class="form-outline mb-4">
          <input v-model="name" type="text" id="addCategoryName" class="form-control bg-light" />
          <label class="form-label" for="addCategoryName">Category's Name</label>
        </div>
        <button type="submit" class="btn btn-primary m-2">Create</button>
        <button v-on:click="switchLoginRegister('main')" 
          class="btn btn-danger right m-2">Cancel</button>
      </form>
    </div>
    <div class="col-md-3" > 
    </div>
  </div>
</div>

</template>

<script>
export default {
  name: "CategoryAddForm",
  data () {
    return {
      name: "",
    }
  },
  methods: {
    handleNameForCreateCategory() {
      let params = this.name
      this.$emit("handleNameForCreateCategory", params)
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