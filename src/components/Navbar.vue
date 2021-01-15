<template>
    <!-- Navbar -->
  <nav>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <!-- Container wrapper -->
      <div class="container-fluid">
        <!-- Navbar brand -->
        <a class="navbar-brand mx-5" style="font-weight:bold;" >KANBAN</a>

        <!-- Toggle button -->
        <button
          class="navbar-toggler floatRight"
          type="button"
          data-mdb-toggle="collapse"
          data-mdb-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <i class="fas fa-bars"></i>
        </button>

        <!-- Collapsible wrapper -->
        <div class="collapse navbar-collapse floatRight" id="navbarSupportedContent"
          v-if="page === 'main' || page === 'addTask' || page === 'addCategory' || page === 'updateForm'" > 
        <!-- Add Category button -->
        <button class="btn text-light colorButtonDark mx-2 floatRight" v-on:click="switchLoginRegister('addCategory')">Add Category</button>
          <!-- Left links -->
          <ul class="navbar-nav me-auto mb-2 mb-lg-0 floatRight">
            <!-- Navbar dropdown -->
            <li class="nav-item dropdown">
              <a
                class="nav-link dropdown-toggle floatRight"
                href=""
                id="navbarDropdown"
                role="button"
                data-mdb-toggle="dropdown"
                aria-expanded="false"
              >
                {{ userFullname }}
              </a>
              <!-- Dropdown menu -->
              <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                 <li><button @click.prevent="signOut" class="btn btn-danger" type="button">Logout</button></li>
              </ul>
            </li>
          </ul>
          <!-- Left links -->
        </div>
        <!-- Collapsible wrapper -->
      </div>
      <!-- Container wrapper -->
    </nav>
    <!-- Navbar -->
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      userFullname: localStorage.getItem("user_fullname")
    }
  },
  methods: {
    signOut() {
      localStorage.clear()
      this.$emit("changePage", "main")
      this.$emit("reCheckAuth")
    },
    reCheckAuth() {
      this.$emit("reCheckAuth")
    }
  },
  props: ["page", "switchLoginRegister"]
}
</script>

<style>
.colorButtonDark {
  background-color: grey;
}
</style>