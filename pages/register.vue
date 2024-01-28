<template>
  <div class="flex items-center justify-center h-screen">
    <div
      class="hidden h-full bg-white rounded-tr-lg rounded-br-lg md:block lg:w-1/3 auth-background"
    ></div>
    <div class="flex items-center justify-center w-auto md:w-2/4 lg:w-2/3">
      <div class="w-full px-10 lg:w-1/2 lg:px-0">
        <h2 class="mb-6 text-3xl font-normal text-white">Sign Up Account</h2>
        <div class="mb-6">
          <div class="mb-4">
            <label class="block mb-3 text-lg font-normal text-white"
              >Full Name</label
            >
            <input
              type="text"
              v-model="register.name"
              class="auth-form focus:outline-none focus:bg-purple-hover focus:shadow-outline focus:border-purple-hover-stroke focus:text-gray-100"
              placeholder="Write Your Name Here"
            />
          </div>
        </div>
        <div class="mb-6">
          <div class="mb-4">
            <label class="block mb-3 text-lg font-normal text-white"
              >Occupation</label
            >
            <input
              type="text"
              v-model="register.occupation"
              class="auth-form focus:outline-none focus:bg-purple-hover focus:shadow-outline focus:border-purple-hover-stroke focus:text-gray-100"
              placeholder="Write your occupation here"
            />
          </div>
        </div>
        <div class="mb-6">
          <div class="mb-4">
            <label class="block mb-3 text-lg font-normal text-white"
              >Email Address</label
            >
            <input
              type="email"
              v-model="register.email"
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
              type="password"
              v-model="register.password"
              class="auth-form focus:outline-none focus:bg-purple-hover focus:shadow-outline focus:border-purple-hover-stroke focus:text-gray-100"
              placeholder="Type your password here"
              @keyup.enter="userRegister()"
            />
          </div>
        </div>
        <div class="mb-6">
          <div class="mb-4">
            <button
              @click="userRegister"
              class="block w-full px-6 py-4 text-lg font-semibold text-white rounded-full bg-orange-button hover:bg-green-button"
            >
              Continue Sign Up
            </button>
          </div>
        </div>
        <div class="text-center">
          <p class="text-white text-md">
            Already have account?
            <nuxt-link to="/login" class="no-underline text-orange-button"
              >Sign In</nuxt-link
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
      register: {
        name: '',
        email: '',
        occupation: '',
        password: '',
      },
    }
  },
  methods: {
    async userRegister() {
      try {
        let response = await this.$axios.post('/api/v1/users', this.register)
        console.log(response.data.data.token)
        this.$auth
          .setUserToken(response.data.data.token)
          .then(() => this.$router.push({ path: '/upload' }))
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style scoped>
.auth-background {
  background-image: url('/sign-up-background.jpg');
  background-position: center;
  background-size: cover;
}
</style>
