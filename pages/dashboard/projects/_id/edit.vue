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
          <h3 class="mb-4 text-2xl text-gray-900">Edit Campaign "{{ campaign.data.name }}"</h3>
        </div>
        <div class="w-1/4 text-right">
          <button
            @click="update"
            class="inline-flex items-center px-4 py-1 font-bold text-white rounded bg-green-button hover:bg-green-button"
          >
            Update
          </button>
        </div>
      </div>
      <div class="block mb-2">
        <div class="w-full mb-4 lg:max-w-full lg:flex">
          <div
            class="flex flex-col justify-between w-full p-8 leading-normal bg-white border border-gray-400 rounded"
          >
            <form class="w-full">
              <div class="flex flex-wrap mb-6 -mx-3">
                <div class="w-full px-3 mb-6 md:w-1/2 md:mb-0">
                  <label
                    class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase"
                  >
                    Campaign Name
                  </label>
                  <input
                    class="block w-full px-4 py-3 leading-tight text-gray-700 bg-gray-200 border border-gray-200 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
                    type="text"
                    placeholder="Contoh: Website Hosting Fundraising"
                    v-model="campaign.data.name"
                  />
                </div>
                <div class="w-full px-3 md:w-1/2">
                  <label
                    class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase"
                  >
                    Price
                  </label>
                  <input
                    class="block w-full px-4 py-3 leading-tight text-gray-700 bg-gray-200 border border-gray-200 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
                    type="number"
                    placeholder="Contoh: 50000000"
                    v-model.number="campaign.data.goal_amount"
                  />
                </div>
                <div class="w-full px-3">
                  <label
                    class="block mt-3 mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase"
                  >
                    Short Description
                  </label>
                  <input
                    class="block w-full px-4 py-3 mb-3 leading-tight text-gray-700 bg-gray-200 border border-gray-200 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
                    type="text"
                    placeholder="Deskripsi singkat mengenai projectmu"
                    v-model="campaign.data.short_description"
                  />
                </div>
                <div class="w-full px-3">
                  <label
                    class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase"
                  >
                    What will backers get
                  </label>
                  <input
                    class="block w-full px-4 py-3 mb-3 leading-tight text-gray-700 bg-gray-200 border border-gray-200 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
                    type="text"
                    placeholder="Contoh: Free hosting, domain, unlimited bandwidth, unlimited storage"
                    v-model="campaign.data.perks"
                  />
                </div>
                <div class="w-full px-3">
                  <label
                    class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase"
                  >
                    Description
                  </label>
                  <textarea
                    class="block w-full px-4 py-3 mb-3 leading-tight text-gray-700 bg-gray-200 border border-gray-200 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
                    type="text"
                    placeholder="Isi deskripsi panjang untuk projectmu"
                    v-model="campaign.data.description"
                  ></textarea>
                </div>
              </div>
            </form>
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
    return { campaign }
  },
  
  methods: {
    async update() {
      try {
        let response = await this.$axios.$put(
          '/api/v1/campaigns/' + this.$route.params.id,
          {
            name: this.campaign.data.name,
            short_description: this.campaign.data.short_description,
            description: this.campaign.data.description,
            goal_amount: this.campaign.data.goal_amount,
            perks: this.campaign.data.perks,
          }
        )
        
        console.log(response)
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
