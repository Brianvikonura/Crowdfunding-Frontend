<template>
  <div class="project-page">
    <section class="px-32 pt-5 project-header">
      <div class="container relative mx-auto">
        <Navbar />
      </div>
    </section>
    <section class="container px-32 mx-auto -mt-56 project-container">
      <div class="flex mt-3">
        <div class="w-3/4 mr-6">
          <div class="p-3 mb-3 bg-white border border-gray-400 rounded-20">
            <figure class="item-image">
              <img :src="default_image" alt="" class="w-full rounded-20" />
            </figure>
          </div>
          <div class="flex -mx-2">
            <div
              v-for="image in campaign.data.images"
              :key="image.image_url"
              class="relative w-1/4 p-2 m-2 bg-white border border-gray-400 rounded-20"
            >
              <figure class="cursor-pointer item-thumbnail">
                <img
                  :src="$axios.defaults.baseURL + '/' + image.image_url"
                  @click="
                    changeImage($axios.defaults.baseURL + '/' + image.image_url)
                  "
                  alt=""
                  class="w-full rounded-20"
                />
              </figure>
            </div>
          </div>
        </div>
        <div class="w-1/4">
          <div
            class="sticky w-full p-5 bg-white border border-gray-400 rounded-20"
            style="top: 15px"
          >
            <h3>Project Leader:</h3>

            <div class="flex mt-3">
              <div class="w-1/4">
                <img
                  :src="
                    $axios.defaults.baseURL + '/' + campaign.data.user.image_url
                  "
                  alt=""
                  class="inline-block w-full rounded-full"
                />
              </div>
              <div class="w-3/4 mt-1 ml-5">
                <div class="text-xl font-semibold text-gray-800">
                  {{ campaign.data.user.name }}
                </div>
                <div class="font-light text-gray-400 text-md">
                  {{ campaign.data.backer_count }} backer
                </div>
              </div>
            </div>

            <h4 class="mt-5 font-semibold">What will you get:</h4>
            <ul class="mt-3 list-check">
              <li v-for="perk in campaign.data.perks" :key="perk">
                {{ perk }}
              </li>
            </ul>
            <template v-if="this.$store.state.auth.loggedIn">
              <input
                type="number"
                class="block w-full px-6 py-3 mt-4 text-gray-800 transition duration-300 ease-in-out border border-gray-500 rounded-full focus:outline-none focus:shadow-outline"
                placeholder="Amount in Rp"
                v-model.number="transactions.amount"
                @keyup.enter="fund"
              />
              <button
                @click="fund"
                class="block w-full px-6 py-3 mt-3 font-medium text-center text-white rounded-full button-cta bg-orange-button hover:bg-green-button text-md"
              >
                Fund Now
              </button>
            </template>
            <template v-else>
              <button
                @click="$router.push({ path: '/login' })"
                class="block w-full px-6 py-3 mt-3 font-medium text-center text-white rounded-full button-cta bg-orange-button hover:bg-green-button text-md"
              >
                Sign in to Fund
              </button>
            </template>
          </div>
        </div>
      </div>
    </section>
    <section class="container px-32 pt-8 mx-auto">
      <div class="flex items-center justify-between">
        <div class="w-full mr-6 md:w-3/4">
          <h2 class="mb-2 text-4xl font-medium text-gray-900">
            {{ campaign.data.name }}
          </h2>
          <p class="mb-5 text-xl font-light">
            {{ campaign.data.short_description }}
          </p>

          <div class="relative progress-bar">
            <div
              class="flex h-6 mb-4 overflow-hidden text-xs bg-gray-200 rounded-full"
            >
              <div
                style="'width: ' + (campaign.data.current_amount / campaign.data.goal_amount) * 100 + '%' "
                class="flex flex-col justify-center text-center text-white shadow-none whitespace-nowrap bg-purple-progress progress-striped"
              ></div>
            </div>
          </div>
          <div class="flex mb-6 progress-info">
            <div class="text-2xl">
              {{
                (campaign.data.current_amount / campaign.data.goal_amount) * 100
              }}
              %
            </div>
            <div class="ml-auto text-2xl font-semibold">
              Rp {{ new Intl.NumberFormat().format(campaign.data.goal_amount) }}
            </div>
          </div>

          <p class="mb-5 text-xl font-light">
            {{ campaign.data.description }}
          </p>
        </div>
        <div class="hidden w-1/4 md:block"></div>
      </div>
    </section>
    <div class="-mt-20 cta-clip"></div>
    <CallToAction />
    <Footer />
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    const campaign = await $axios.$get('/api/v1/campaigns/' + params.id)
    return { campaign }
  },
  data() {
    return {
      default_image: '',
      transactions: {
        amount: 0,
        campaign_id: Number.parseInt(this.$route.params.id),
        code: ''
      },
    }
  },
  methods: {
    changeImage(url) {
      this.default_image = url
    },
    async fund() {
      try {
        let response = await this.$axios.post(
          '/api/v1/transactions',
          this.transactions
        )
        window.location = response.data.data.payment_url
        console.log(response.data.data.payment_url)
      } catch (error) {
        console.log(error)
      }
    },
  },
  mounted() {
    this.default_image =
      this.$axios.defaults.baseURL + '/' + this.campaign.data.image_url
  },
}
</script>
