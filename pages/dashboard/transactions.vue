<template>
  <div class="project-page">
    <section class="px-32 pt-5 dashboard-header">
      <div class="container relative mx-auto">
        <Navbar />
      </div>
    </section>
    <section class="container px-32 pt-8 mx-auto">
      <div class="flex items-center justify-between mb-6">
        <div class="w-3/4 mr-6">
          <h2 class="mb-2 text-4xl font-medium text-gray-900">Dashboard</h2>
          <ul class="flex mt-2">
            <li class="mr-6">
              <nuxt-link
                class="text-gray-500 hover:text-gray-800"
                to="/dashboard"
              >
                Your Projects
              </nuxt-link>
            </li>
            <li class="mr-6">
              <nuxt-link
                class="font-bold text-gray-800"
                to="/dashboard/transactions"
              >
                Your Transactions
              </nuxt-link>
            </li>
          </ul>
        </div>
      </div>
      <hr />
      <div class="block mb-2">
        <div
          class="w-full mb-4 lg:max-w-full lg:flex"
          v-for="transaction in transactions.data"
          :key="transaction.id"
        >
          <div
            class="flex-none h-48 overflow-hidden text-center bg-cover rounded-t lg:h-auto lg:w-48 lg:rounded-t-none lg:rounded-l"
            :style="
              'background-color: #bbb; background-position: center; background-image: url(\'' +
              $axios.defaults.baseURL +
              '/' +
              transaction.campaign.image_url +
              '\')'
            "
          ></div>
          <div
            class="flex flex-col justify-between w-full p-8 leading-normal bg-white border-b border-l border-r border-gray-400 rounded-b lg:border-l-0 lg:border-t lg:border-gray-400 lg:rounded-b-none lg:rounded-r"
          >
            <div>
              <div class="mb-1 text-xl font-bold text-gray-900">
                {{ transaction.campaign.name }}
              </div>
              <p class="flex items-center mb-2 text-sm text-gray-600">
                Rp
                {{ new Intl.NumberFormat().format(transaction.amount) }}
                &middot; {{ transaction.created_at }}
                &middot; {{ transaction.status }}
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
  async asyncData({ $axios, app }) {
    const transactions = await $axios.$get('/api/v1/transactions')
    return { transactions }
  },
}
</script>
