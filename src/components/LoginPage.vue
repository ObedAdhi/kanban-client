<template>

  <!-- Login Card -->
  <div class="container-fluid mt-5">
    <div class="d-flex justify-content-center">
      <div class="d-inline-flex p-2" style="width: 400px; height: 500px;">
        <div class="card text-center" style="width: 400px; background-color: #eeeeee;">
          <div class="card-header">
            <ul class="nav nav-tabs card-header-tabs">
              <li class="nav-item">
                <button class="nav-link active btn btn-primary" aria-current="true" 
                v-on:click.prevent="switchLoginRegister('login')">Login</button>
              </li>
              <li class="nav-item">
                <button class="nav-link btn" aria-current="true" 
                v-on:click.prevent="switchLoginRegister('register')">Register</button>
              </li>
            </ul>
          </div>
          <div class="card-body">
            <h5 class="card-title">KANBAN</h5>
            <form @submit.prevent="handleLoginPackage">
              <!-- Email input -->
              <div class="form-outline mb-4">
                <input v-model="email" type="email" id="form3Example3" class="form-control bg-light" />
                <label class="form-label focus"  for="form3Example3">Email address</label>
              </div>
            
              <!-- Password input -->
              <div class="form-outline mb-4">
                <input v-model="password" type="password" id="form3Example4" class="form-control bg-light" />
                <label class="form-label" for="form3Example4">Password</label>
              </div>
            
              <!-- Submit button -->
              <button type="submit" class="btn btn-primary btn-block mb-4">Login</button>
            </form>
            <p>Or login using:</p>

            <button v-google-signin-button="clientId" class="google-signin-button">Google</button>
            
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- Login Card -->

</template>

<script>
import axios from "axios"
import GoogleSignInButton from 'vue-google-signin-button-directive'

export default {
  name: "LoginPage",
    directives: {
    GoogleSignInButton
  },
  data() {
    return {
      clientId: '775560134433-cep59p1uhqf8q4p8jsr9gfl2gbefddep.apps.googleusercontent.com',
      email: "",
      password: "",
      fullname: "",
      server: "https://kanban-server-wary-fox.herokuapp.com",
    }
  },
  methods: {
    handleLoginPackage() {
      let email = this.email
      let password = this.password
      let packet = {
        email,
        password
      }
      this.$emit("handleLoginPackage", packet)
    },

    OnGoogleAuthSuccess (idToken) {
      // Receive the idToken and make your magic with the backend
      console.log(idToken);
      let id_token = idToken
      axios({
        method: "POST",
        url: `${this.server}/googleLogin`,
        data: {
          id_token
        },
      })
      .then(data => {
        console.log(data);
        localStorage.setItem("access_token", data.data.access_token)
        localStorage.setItem("user_email", data.data.email)
        localStorage.setItem("user_fullname", data.data.fullname)
        this.$emit("changePage", "main")
        this.$emit("reCheckAuth")
      })
      .catch(err => {
        console.log(err);
      })
    },
    OnGoogleAuthFail (error) {
      console.log(error)
    },

  },
  props: ["page", "switchLoginRegister"]

}
</script>

<style>
.google-signin-button {
  color: blue;
  background-color: #eeeeee;
  height: 50px;
  font-size: 16px;
  border-radius: 10px;
  padding: 10px 20px 25px 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>