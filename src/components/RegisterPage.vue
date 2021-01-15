<template>
    <!-- Register Card -->
    <div class="container-fluid mt-5">
      <div class="d-flex justify-content-center">
        <div class="d-inline-flex p-2" style="width: 400px; height: 500px;">
          <div class="card text-center"  style="width: 400px; background-color: #eeeeee;">
            <div class="card-header">
              <ul class="nav nav-tabs card-header-tabs">
                <li class="nav-item">
                  <button class="nav-link btn" aria-current="true" 
                  v-on:click="switchLoginRegister('login')">Login</button>
                </li>
                <li class="nav-item">
                  <button class="nav-link active btn btn-primary" aria-current="true" 
                  v-on:click="switchLoginRegister('register')">Register</button>
                </li>
              </ul>
            </div>
            <div class="card-body">
              <h5 class="card-title">KANBAN</h5>
              <form @submit.prevent="handleRegisterData">
                <!-- fullname -->
                <div class="row mb-4">
                  <div class="col">
                    <div class="form-outline">
                      <input v-model="fullname" type="text" id="form3Example1"  class="form-control bg-light" />
                      <label class="form-label" for="form3Example1">Fullname</label>
                    </div>
                  </div>
                </div>
              
                <!-- Email input -->
                <div class="form-outline mb-4">
                  <input v-model="email" type="email" id="form3Example3"  class="form-control bg-light" />
                  <label class="form-label" for="form3Example3">Email address</label>
                </div>
              
                <!-- Password input -->
                <div class="form-outline mb-4">
                  <input v-model="password" type="password" id="form3Example4"  class="form-control bg-light" />
                  <label class="form-label" for="form3Example4">Password</label>
                </div>
              
                <!-- Submit button -->
                <button type="submit" class="btn btn-primary btn-block mb-4">Register</button>
              </form>
            </div>
          </div>  
        </div>
      </div>
    </div>
    <!-- Register Card -->
</template>

<script>
export default {
  name: "RegisterPage",
  data() {
    return {
      email: "",
      password: "",
      fullname: "",
      server: "http://localhost:3000",
    }
  },
  methods: {
    handleRegisterData(){
      let fullname = this.fullname
      let email = this.email
      let password = this.password
      let packet = {
        fullname,
        email,
        password
      }
      this.$emit("handleRegisterData", packet)
    },
  },
  props: ["page", "switchLoginRegister"],
}
</script>

<style>

</style>