<template>
  <header class="flex items-center">
    <div style="height: 54px" class="pr-5">
      <img src="/logo.svg" alt="logo" class="h-full" />
    </div>
    <ul class="flex items-center">
      <li>
        <nuxt-link
          class="px-4 py-3 text-lg text-white hover:text-teal-500"
          to="/"
          >Home</nuxt-link
        >
      </li>
      <li>
        <nuxt-link
          class="px-4 py-3 text-lg text-white hover:text-teal-500"
          to="/projects"
          >Project</nuxt-link
        >
      </li>
      <li>
        <nuxt-link
          class="px-4 py-3 text-lg text-white hover:text-teal-500"
          to="/features"
          >Features</nuxt-link
        >
      </li>
      <li>
        <nuxt-link
          class="px-4 py-3 text-lg text-white hover:text-teal-500"
          to="/story"
          >Success Stories</nuxt-link
        >
      </li>
    </ul>
    <ul
      class="flex items-center mt-2 ml-auto"
      v-if="!this.$store.state.auth.loggedIn"
    >
      <li>
        <nuxt-link
          to="/register"
          class="inline-block w-40 px-6 py-1 mr-4 text-lg font-light text-center text-white bg-transparent border border-white rounded-full hover:bg-white hover:bg-opacity-25"
        >
          Sign Up
        </nuxt-link>
      </li>
      <li>
        <nuxt-link
          to="/login"
          class="inline-block w-40 px-6 py-1 text-lg font-light text-center text-white bg-transparent border border-white rounded-full hover:bg-white hover:bg-opacity-25"
        >
          My Account
        </nuxt-link>
      </li>
    </ul>
    <div class="flex ml-auto" v-else>
      <div class="relative z-10 inline-block dropdown">
        <button
          class="inline-flex items-center px-6 py-4 font-semibold text-gray-700 bg-white rounded"
        >
          <img
            v-if="$store.state.auth.user.image_url"
            :src="
              $axios.defaults.baseURL + '/' + $store.state.auth.user.image_url
            "
            alt=""
            class="h-8 mr-2 rounded-full"
          />
          <span class="mr-1">
            {{ this.$store.state.auth.user.name }}
          </span>
          <svg
            class="w-4 h-4 fill-current"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
          >
            <path
              d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
            />
          </svg>
        </button>
        <ul
          class="absolute hidden w-full pt-1 -mt-2 text-gray-700 shadow dropdown-menu"
        >
          <li>
            <nuxt-link
              to="/dashboard"
              class="block px-4 py-2 bg-white whitespace-nowrap hover:bg-gray-100 hover:text-yellow-600"
            >
              My Dashboard
            </nuxt-link>
          </li>
          <li>
            <nuxt-link
              to="/dashboard"
              class="block px-4 py-2 bg-white whitespace-nowrap hover:bg-gray-100 hover:text-yellow-600"
            >
              Account Settings
            </nuxt-link>
          </li>
          <li>
            <a
              @click="logout()"
              class="block px-4 py-2 bg-white rounded-b cursor-pointer whitespace-nowrap hover:bg-gray-100 hover:text-yellow-600"
            >
              logout
            </a>
          </li>
        </ul>
      </div>
    </div>
  </header>
</template>

<style scoped>
.dropdown:hover .dropdown-menu {
  display: block;
}
</style>

<script>
export default {
  methods: {
    async logout () {
      await this.$auth.logout()
    }
  }
}
</script>