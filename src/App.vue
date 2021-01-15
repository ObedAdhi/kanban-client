<template>
  <div>
    <Navbar 
      v-if=" page !== 'null'"
      :page="page"
      :switchLoginRegister="switchLoginRegister"
      @reCheckAuth="checkAuth">
    </Navbar>
    <div>
      <LoginPage v-if=" page === 'login'" 
        :page="page" 
        :switchLoginRegister="switchLoginRegister"
        :clientId="clientId"
        @changePage="switchLoginRegister"
        @handleLoginPackage="loginNormal"
        @reCheckAuth="checkAuth">
      </LoginPage>

      <RegisterPage v-if=" page === 'register'" 
        :page="page" :switchLoginRegister="switchLoginRegister"
        @changePage="switchLoginRegister"
        @handleRegisterData="register">
      </RegisterPage>

      <MainPage 
        v-if=" page === 'main'"
        :allCategory="allCategory"
        @getCategoryId="getCategoryId"
        @handleTaskId="deleteTask"
        @handleTaskIdForUpdate="getTaskIdForUpdate"
        @handleTaskData="changeTaskCategory"
        @handleDeleteCategory="deleteCategory">
      </MainPage>

      <TaskAddForm
        v-if=" page === 'addTask'"
        :switchLoginRegister="switchLoginRegister"
        @changePage="switchLoginRegister"
        @carryTitle="createTask">
      </TaskAddForm>

      <TaskUpdateForm
        v-if=" page === 'updateForm'"
        :switchLoginRegister="switchLoginRegister"
        @handleTitleForUpdateTask="updateOneTask">
      </TaskUpdateForm>

      <CategoryAddForm
        v-if=" page === 'addCategory'" 
        :switchLoginRegister="switchLoginRegister"
        @changePage="switchLoginRegister"
        @handleNameForCreateCategory="createCategory">
      </CategoryAddForm>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import Swal from 'sweetalert2'
import GoogleSignInButton from 'vue-google-signin-button-directive'
import LoginPage from "./components/LoginPage"
import MainPage from './components/MainPage.vue'
import Navbar from './components/Navbar.vue'
import RegisterPage from "./components/RegisterPage"
import TaskAddForm from "./components/TaskAddForm"
import TaskUpdateForm from "./components/TaskUpdateForm"
import CategoryAddForm from "./components/CategoryAddForm"

export default {
  name: "App",
  directives: {
    GoogleSignInButton
  },
  data() {
    return {
      clientId: '775560134433-nkbdmm81lau6onegdlk9ek9to5nf6bia.apps.googleusercontent.com',
      message: "Hello vue",
      page: "login",
      server: "https://kanban-server-wary-fox.herokuapp.com",
      allTask: [],
      allCategory: [],
      email: "",
      password: "",
      fullname: "",
      categoryIdForCreatingTask: "",
      taskIdForUpdatingTask: "",
    }
  },
  components: {
    LoginPage,
    RegisterPage,
    MainPage,
    Navbar,
    TaskAddForm,
    TaskUpdateForm,
    CategoryAddForm,
  },
  methods: {
    checkAuth() {
      if(localStorage.getItem("access_token")) {
        this.page = "main"
        this.getAllCategory()
      } else {
        this.page = "login"
      }
    },

    switchLoginRegister(page) {
      this.page = page
    },

    register(packet){
      let {
        fullname,
        email,
        password
      } = packet
      axios({
        method: "POST",
        url: `${this.server}/register`,
        data: {
          fullname,
          email,
          password
        },
      })
      .then(data => {
        console.log(data.data);
        localStorage.setItem("access_token", data.data.access_token)
        localStorage.setItem("user_email", data.data.email)
        localStorage.setItem("user_fullname", data.data.fullname)
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response);
        if (err.response.data.message === "Email already used") {
          Swal.fire({
            title: 'Error!',
            text: err.response.data.message,
            icon: 'error',
          })
        }
        Swal.fire({
          title: 'Error!',
          text: err.response.data[0].message || err.response.data.message,
          icon: 'error',
        })
      })
    },

    loginNormal(packet) {
      let {
        email,
        password
      } = packet
      axios({
        method: "POST",
        url: `${this.server}/login`,
        data: {
          email,
          password
        },
      })
      .then(data => {
        console.log(data.data);
        localStorage.setItem("access_token", data.data.access_token)
        localStorage.setItem("user_email", data.data.email)
        localStorage.setItem("user_fullname", data.data.fullname)
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response);
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },

    getAllTask() {
      axios({
        method: "GET",
        url: this.server + "/tasks",
        headers: {
          access_token: localStorage.access_token
        }
      })
      .then(data => {
        console.log(data.data);
        this.allTask = data.data
      })
      .catch(err => {
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },
    
    getAllCategory() {
      axios({
        method: "GET",
        url: this.server + "/categories",
        headers: {
          access_token: localStorage.access_token
        }
      })
      .then(data => {
        this.allCategory = data.data
      })
      .catch(err => {
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },

    getCategoryId(id) { //for creating task
      this.categoryIdForCreatingTask = id
      this.switchLoginRegister("addTask")
      console.log(id + "<<<<<< dari add task");
    },

    getTaskIdForUpdate(id) {
      this.taskIdForUpdatingTask = id
      this.switchLoginRegister("updateForm")
    },

    createTask(titleParams) {
      let title = titleParams
      let categoryId = this.categoryIdForCreatingTask
      axios({
        method: "POST",
        url: this.server + "/tasks",
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          title,
          categoryId
        }
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response);
        Swal.fire({
          title: 'Error!',
          text: err.response.data[0].message,
          icon: 'error',
        })
      })
    },

    createCategory(nameParams) {
      let name = nameParams
      axios({
        method: "POST",
        url: this.server + "/categories",
        headers: {
          access_token: localStorage.access_token
        }, 
        data: {
          name
        }
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        Swal.fire({
          title: 'Error!',
          text: err.response.data[0].message,
          icon: 'error',
        })
      })
    },

    updateOneTask (titleParams) {
      let title = titleParams
      let id = this.taskIdForUpdatingTask
      axios({
        method: "PUT",
        url: this.server + "/tasks/" + id,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          title
        }
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },
 
    changeTaskCategory (params) {
      let taskId = params.id
      let categoryId = params.categoryId
      let storeCategoryId = this.allCategory
      let idForUpdateTask = ""
      for (let i = 0; i < storeCategoryId.length; i++) {
        const element = storeCategoryId[i];
        if (element.id === categoryId) {
          if (i !== storeCategoryId.length-1) {
            idForUpdateTask = storeCategoryId[i + 1].id 
            break
          }
        } else {
          idForUpdateTask = storeCategoryId[0].id
        }
      }
      console.log(idForUpdateTask + "<<<<<< move category");
      
      axios({
        method: "PATCH",
        url: this.server + "/tasks/" + taskId,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          categoryId: idForUpdateTask
        }
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response.data);
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },

    deleteTask(idParam) {
      axios({
        method: "DELETE",
        url: this.server + "/tasks/" + idParam,
        headers: {
          access_token: localStorage.access_token
        },
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response.data);
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },

    deleteCategory(idParam) {
      console.log(idParam);
      axios({
        method: "DELETE",
        url: this.server + "/categories/" + idParam,
        headers: {
          access_token: localStorage.access_token
        },
      })
      .then(data => {
        this.checkAuth()
      })
      .catch(err => {
        console.log(err.response.data);
        Swal.fire({
          title: 'Error!',
          text: err.response.data.message,
          icon: 'error',
        })
      })
    },



  },

  created() {
    this.checkAuth()
  },
  mounted() {
    this.checkAuth()
    if(localStorage.getItem("access_token")) {
      this.getAllCategory()
    }
    this.checkAuth()
  }
}
</script>

<style>

</style>