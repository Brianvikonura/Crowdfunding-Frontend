<template>
  <div class="container flex items-center justify-center h-screen mx-auto">
    <div class="w-full px-10 lg:w-1/3 lg:px-0">
      <div class="flex items-center justify-center w-40 mx-auto mb-4">
        <div class="relative">
          <div class="cursor-pointer" @click="$refs.file.click()">
            <img
              :src="url"
              alt=""
              class="border-4 border-white rounded-full"
            />
            <img
              src="/icon-avatar-add.svg"
              alt=""
              class="absolute bottom-0 right-0 pb-2"
            />
            <input type="file" ref="file" style="display: none" accept="image/*" @change="onFileChange">
          </div>
        </div>
      </div>
      <h2 class="mb-3 text-3xl font-normal text-center text-white">
        Hi, {{ this.$store.state.auth.user.name }}
      </h2>
      <p class="font-light text-center text-white">Please upload your selfie</p>
      <div class="mt-6 mb-4">
        <div class="mb-3 mx-9">
          <button
            :disabled="selectedFiles ==undefined"
            @click="upload"
            :class="selectedFiles == undefined ? 'opacity-50 cursor-not-allowed' : ''"
            class="block w-full px-6 py-4 text-lg font-semibold text-white rounded-full bg-orange-button hover:bg-green-button"
          >
            Sign Up Now
          </button>
        </div>
      </div>
      <div>
        <div class="mb-4 mx-9">
          <button
            @click="$router.push({ path: '/register-success' })"
            class="block w-full px-6 py-4 text-lg font-light text-white bg-transparent border border-white rounded-full hover:bg-white hover:bg-opacity-25"
          >
            Skip
          </button>
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
      url: '/avatar.jpg',
      selectedFiles: undefined,
    }
  },
  methods: {
    onFileChange(e) {
      const file = e.target.files[0]
      this.url = URL.createObjectURL(file)
      this.selectedFiles = this.$refs.file.files
    },
    async upload(file) {
      let formData = new FormData()

      formData.append('avatar', this.selectedFiles.item(0))

      try {
        let response = await this.$axios.post('/api/v1/avatars', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        console.log(response)

        this.$router.push({ path: '/register-success' })
      } catch (error) {
        console.log(error)
      }
    }
  },
}
</script>
