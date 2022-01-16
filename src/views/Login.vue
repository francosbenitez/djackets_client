<template>
  <div>
    <h1>Log In</h1>

    <form @submit.prevent="submitForm">
      <div class="">
        <label>Username</label>
        <input type="text" v-model="username">
      </div>

      <div class="">
        <label>Password</label>
        <input type="text" v-model="password">
      </div>

      <!-- <div class="">
        <label>Repeat password</label>
        <input type="text" v-model="password2">
      </div> -->

      <div class="" v-if="errors.length">
        <p v-for="error in errors" :key="error">
          {{ error }}
        </p>
      </div>

      <div class="">
        <button>Log In</button>
      </div>

      <hr>

      Or 
      <router-link to="/sign-up">
        click here
      </router-link>
      to sign up!
    </form>

  </div> 
</template>

<script>
import axios from 'axios'

export default {
  name: 'LogIn',
  data() {
    return {
      username: '',
      password: '',
      errors: []
    }
  },
  mounted() {
    document.title = 'Log In | Djackets'
  },
  methods: {
    async submitForm() {
      axios.defaults.headers.common["Authorization"] = ""
      localStorage.removeItem("token")
      const formData = {
        username: this.username,
        password: this.password
      }
      await axios
        .post("/api/v1/token/login/", formData)
        .then(response => {
          const token = response.data.auth_token
          this.$store.commit('setToken', token)
          
          axios.defaults.headers.common["Authorization"] = "Token " + token
          localStorage.setItem("token", token)
          const toPath = this.$route.query.to || '/cart'
          this.$router.push(toPath)
        })
        .catch(error => {
          if (error.response) {
            for (const property in error.response.data) {
              this.errors.push(`${property}: ${error.response.data[property]}`)
            }
          } else {
            this.errors.push('Something went wrong. Please try again')
              
            console.log(JSON.stringify(error))
          }
        })
    }
  }
}
</script>
