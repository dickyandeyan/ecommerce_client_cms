<template>
  <div class="card-login">
    <div class="card">
      <div class="card-body">
        <h1 class="text-center text-muted">
          <i class="fa fa-lock"></i>
        </h1>
        <h3>CMS E-commerce</h3>
        <small id="emailHelp" class="form-text text-center text-muted mb-2"
          >Login with admin account to access CMS</small
        >
        <ErrorMsg :errorMsg="errorMsg" v-if="isError" />
        <form @submit.prevent="userLogin">
          <div class="form-group">
            <input
              v-model="email"
              type="email"
              class="form-control"
              id="email"
              aria-describedby="emailHelp"
              placeholder="Email"
              autocomplete="off"
              required
            />
          </div>
          <div class="form-group">
            <input
              v-model="password"
              type="password"
              class="form-control"
              id="password"
              placeholder="Password"
            />
          </div>
          <button type="submit" class="btn btn-block" v-if="!isLoad">
            Login
          </button>
          <!-- <p
            class="text-center"
            v-else-if="isLoad"
          >Authenticating...</p> -->
          <div v-else-if="isLoad" class="text-center">
            <div v class="spinner-border" role="status">
              <span class="sr-only">Loading...</span>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import ErrorMsg from '@/components/ErrorMsg.vue'
export default {
  name: 'LoginCard',
  data () {
    return {
      email: '',
      password: '',
      errorMsg: '',
      isLoad: false,
      isError: false
    }
  },
  components: {
    ErrorMsg
  },
  methods: {
    userLogin () {
      this.isLoad = true
      this.isError = false
      const payload = {
        email: this.email,
        password: this.password
      }
      this.$store.dispatch('userLogin', payload)
        .then(({ data }) => {
          localStorage.setItem('token', data.access_token)
          localStorage.setItem('full_name', data.full_name)
          localStorage.setItem('email', data.email)
          this.$router.push('/dashboard')
        })
        .catch(err => {
          this.isError = true
          this.errorMsg = err.response.data.message
        })
        .finally(() => {
          this.isLoad = false
        })
    }
  }
}
</script>

<style></style>
