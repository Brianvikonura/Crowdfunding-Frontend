<template>
  <div class="flex items-center justify-center h-screen">
    <div
      class="hidden h-full bg-white rounded-tr-lg rounded-br-lg md:block lg:w-1/3 auth-background"
    ></div>
    <div class="flex items-center justify-center w-auto md:w-2/4 lg:w-2/3">
      <div class="w-full px-10 lg:w-1/2 lg:px-0">
        <h2 class="mb-6 text-3xl font-normal text-white">
          Sign In to Your Account
        </h2>
        <div class="mb-6">
          <div class="mb-4">
            <label class="block mb-3 text-lg font-normal text-white"
              >Email Address</label
            >
            <input
              type="email"
              v-model="login.email"
              class="auth-form focus:outline-none focus:bg-purple-hover focus:shadow-outline focus:border-purple-hover-stroke focus:text-gray-100"
              placeholder="Write your email address here"
            />
          </div>
        </div>
        <div class="mb-6">
          <div class="mb-4">
            <label class="block mb-3 text-lg font-normal text-white"
              >Password</label
            >
            <input
              @keyup.enter="userLogin()"
              type="password"
              v-model="login.password"
              class="auth-form focus:outline-none focus:bg-purple-hover focus:shadow-outline focus:border-purple-hover-stroke focus:text-gray-100"
              placeholder="Write your password here"
            />
          </div>
        </div>
        <div class="mb-6">
          <div class="mb-4">
            <button
              @click="userLogin()"
              class="block w-full px-6 py-4 text-lg font-semibold text-white rounded-full bg-orange-button hover:bg-green-button"
            >
              Sign In
            </button>
          </div>
        </div>
        <div class="text-center">
          <p class="text-white text-md">
            Don't have account?
            <nuxt-link to="/register" class="no-underline text-orange-button"
              >Sign Up</nuxt-link
            >.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'auth',
  data() {
    return {
      login: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    async userLogin() {
      try {
        let response = await this.$auth.loginWith('local', {
          data: this.login
        })
        this.$auth.setUser(response.data.data)
        console.log(response)
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style scoped>
.auth-background {
  background-image: url("/sign-in-background.jpg");
  background-position: center;
  background-size: cover;
}
</style>