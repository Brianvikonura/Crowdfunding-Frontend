<template>
  <div class="project-page">
    <section class="px-32 pt-5 dashboard-header">
      <div class="container relative mx-auto">
        <Navbar />
      </div>
    </section>
    <section class="container px-32 pt-8 mx-auto">
      <div class="flex items-center justify-between">
        <div class="w-full mr-6">
          <h2 class="mb-2 text-4xl font-medium text-gray-900">Dashboard</h2>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <div class="w-3/4 mr-6">
          <h3 class="mb-4 text-2xl text-gray-900">Campaign Details</h3>
        </div>
        <div class="w-1/4 text-right">
          <nuxt-link
            :to="{
              name: 'dashboard-projects-id-edit',
              params: { id: campaign.data.id },
            }"
            class="inline-flex items-center px-4 py-1 font-bold text-white rounded bg-green-button hover:bg-green-button"
          >
            Edit
          </nuxt-link>
        </div>
      </div>
      <div class="block mb-2">
        <div class="w-full mb-4 lg:max-w-full lg:flex">
          <div
            class="flex flex-col justify-between p-8 leading-normal bg-white border border-gray-400 rounded"
          >
            <div>
              <div class="mb-2 text-xl font-bold text-gray-900">
                {{ campaign.data.name }}
              </div>
              <p class="flex items-center mb-1 text-sm font-bold">
                Short Description
              </p>
              <p class="text-base text-gray-700">
                {{ campaign.data.short_description }}
              </p>
              <p class="flex items-center mt-3 mb-1 text-sm font-bold">
                Description
              </p>
              <p class="text-base text-gray-700">
                {{ campaign.data.description }}
              </p>
              <p class="flex items-center mt-4 mb-1 text-sm font-bold">
                What Will Funders Get
              </p>
              <ul class="ml-5 list-disc">
                <li v-for="perk in campaign.data.perks" :key="perk">
                  {{ perk }}
                </li>
              </ul>
              <p class="flex items-center mt-4 mb-1 text-sm font-bold">
                Goal amount
              </p>
              <p class="text-4xl text-gray-700">
                Rp.
                {{ new Intl.NumberFormat().format(campaign.data.goal_amount) }}
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <div class="w-2/4 mr-6">
          <h3 class="mt-5 mb-4 text-2xl text-gray-900">Gallery</h3>
        </div>
        <div class="w-2/4 text-right">
          <input
            type="file"
            ref="file"
            @change="selectFile"
            class="p-1 overflow-hidden border rounded"
          />
          <button
            @click="upload"
            class="inline-flex items-center px-4 py-2 font-bold text-white rounded bg-green-button hover:bg-green-button"
          >
            Upload
          </button>
        </div>
      </div>
      <div class="grid grid-cols-4 gap-4 -mx-2">
        <div
          class="relative w-full p-2 m-2 bg-white border border-gray-400 rounded"
          v-for="image in campaign.data.images"
          :key="image.image_url"
        >
          <figure class="item-thumbnail">
            <img
              :src="$axios.defaults.baseURL + '/' + image.image_url"
              alt=""
              class="w-full rounded"
            />
          </figure>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <div class="w-3/4 mr-6">
          <h3 class="mt-5 mb-4 text-2xl text-gray-900">Transaction History</h3>
        </div>
      </div>
      <div class="block mb-2">
        <div
          class="w-full mb-4 lg:max-w-full lg:flex"
          v-for="transaction in transactions.data"
          :key="transaction.id"
        >
          <div
            class="flex flex-col justify-between w-full p-8 leading-normal bg-white border border-gray-400 rounded lg:border-gray-400"
          >
            <div>
              <div class="mb-1 text-xl font-bold text-gray-900">
                {{ transaction.name }}
              </div>
              <p class="flex items-center mb-2 text-sm text-gray-600">
                Rp.
                {{ new Intl.NumberFormat().format(transaction.amount) }}
                &middot; {{ transaction.created_at }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>
    <div class="-mt-20 cta-clip"></div>
    <section class="pt-64 pb-10 call-to-action bg-purple-progress"></section>
    <Footer />
  </div>
</template>

<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios, params }) {
    const campaign = await $axios.$get('/api/v1/campaigns/' + params.id)
    const transactions = await $axios.$get(
      '/api/v1/campaigns/' + params.id + '/transactions'
    )

    return { campaign, transactions }
  },

  data() {
    return {
      selectedFiles: undefined,
    }
  },

  methods: {
    selectFile() {
      this.selectedFiles = this.$refs.file.files
    },
    async load() {
      const campaign = await this.$axios.$get(
        '/api/v1/campaigns/' + this.$route.params.id
      )
      this.campaign = campaign
    },
    async upload(file) {
      let formData = new FormData()

      formData.append('campaign_id', this.$route.params.id)
      formData.append('file', this.selectedFiles.item(0))
      formData.append('is_primary', true)

      try {
        let response = await this.$axios.post(
          '/api/v1/campaign-images',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data',
            },
          }
        )
        console.log(response)

        this.load()
        this.selectFiles = undefined
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>
