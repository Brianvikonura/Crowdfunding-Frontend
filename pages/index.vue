<template>
  <div class="landing-page">
    <section class="px-32 pt-5 landing-hero">
      <div class="header__bg"></div>
      <div class="container relative mx-auto">
        <Navbar />
        <div class="flex items-center px-5 pt-10 md:px-0">
          <div class="w-1/2">
            <h1 class="mb-3 text-4xl text-white">
              We helps <u class="hero-underline">startup</u> to <br />
            </h1>
            <h1 class="mb-3 text-4xl text-white">
              getting started & <u class="hero-underline">funding</u> <br />
            </h1>
            <h1 class="mb-5 text-4xl text-white">their truly needs</h1>
            <p class="mb-8 text-xl font-light text-white">
              Fund the best idea to become <br />
              a real product and be the contributor
            </p>
            <button
              @click="$router.push({ path: '/' })"
              class="block px-12 py-3 text-xl font-semibold text-white rounded-full bg-orange-button hover:bg-green-button"
            >
              Find a Project
            </button>
          </div>
          <div class="flex justify-center w-1/2">
            <img src="/hero-image@2x.png" alt="crowdfunding project" />
          </div>
        </div>
      </div>
    </section>
    <section class="container px-32 pt-24 mx-auto">
      <div class="flex items-center justify-between mb-10">
        <div class="w-auto">
          <h2 class="mb-8 text-3xl text-gray-900">
            Only 3 steps to execute <br />
            your bright ideas
          </h2>
        </div>
      </div>
      <div class="flex">
        <div class="w-full px-56 mb-5">
          <img src="/line-step.svg" alt="" class="w-full" />
        </div>
      </div>
      <div class="flex items-center justify-between text-center">
        <div class="w-1/3">
          <figure class="flex items-center justify-center">
            <img src="/step-1-illustration.svg" alt="" class="mb-8 h-30" />
          </figure>
          <div class="step-content">
            <h3 class="font-medium">Sign Up</h3>
            <p class="font-light">
              Sign Up account and start <br />funding project
            </p>
          </div>
        </div>
        <div class="w-1/3">
          <figure class="flex items-center justify-center -mt-24">
            <img src="/step-2-illustration.svg" alt="" class="mb-8 h-30" />
          </figure>
          <div class="step-content">
            <h3 class="font-medium">Open Project</h3>
            <p class="font-light">
              Choose some project idea, <br />
              and start funding
            </p>
          </div>
        </div>
        <div class="w-1/3">
          <figure class="flex items-center justify-center -mt-48">
            <img src="/step-3-illustration.svg" alt="" class="mb-8 h-30" />
          </figure>
          <div class="step-content">
            <h3 class="font-medium">Execute</h3>
            <p class="font-light">
              Time to makes dream <br />
              comes true
            </p>
          </div>
        </div>
      </div>
    </section>
    <section class="container px-32 pt-24 mx-auto">
      <div class="flex items-center justify-between">
        <div class="w-auto">
          <h2 class="mb-8 text-3xl text-gray-900">
            New projects you can <br />
            taken care of
          </h2>
        </div>
        <div class="w-auto mt-5">
          <a class="font-medium text-gray-900 hover:underline text-md" href=""
            >View All</a
          >
        </div>
      </div>
      <div class="grid grid-cols-3 gap-4 mt-3">
        <div
          v-for="campaign in campaigns.data"
          :key="campaign.id"
          class="w-full p-5 border border-gray-500 card-project rounded-20"
        >
          <div class="item">
            <figure class="item-image">
              <img
                :src="$axios.defaults.baseURL + '/' + campaign.image_url"
                alt=""
                class="w-full rounded-20"
              />
            </figure>
            <div class="item-meta">
              <h4 class="mt-5 text-3xl font-medium text-gray-900">
                {{ campaign.name }}
              </h4>
              <p class="h-12 font-light text-gray-900 text-md">
                {{ campaign.short_description }}
              </p>
              <div class="relative pt-4 progress-bar">
                <div
                  class="flex h-2 h-3 mb-4 overflow-hidden text-xs bg-gray-200 rounded rounded-lg"
                >
                  <div
                    style="'width: ' + (campaign.current_amount / campaign.goal_amount) * 100 + '%' "
                    class="flex flex-col justify-center text-center text-white shadow-none whitespace-nowrap bg-purple-progress progress-striped"
                  ></div>
                </div>
              </div>
              <div class="flex progress-info">
                <div>
                  {{ (campaign.current_amount / campaign.goal_amount) * 100 }} %
                </div>
                <div class="ml-auto font-semibold">
                  Rp {{ new Intl.NumberFormat().format(campaign.goal_amount) }}
                </div>
              </div>
            </div>
            <button
              @click="
                $router.push({
                  name: 'projects-id',
                  params: { id: campaign.id },
                })
              "
              class="block w-full px-6 py-2 mt-5 text-lg font-semibold text-center text-white rounded-full button-cta bg-orange-button hover:bg-green-button"
            >
              Fund Now
            </button>
          </div>
        </div>
      </div>
    </section>
    <section class="container px-32 pt-24 mx-auto">
      <div class="flex items-center justify-between">
        <div class="w-auto">
          <h2 class="mb-8 text-3xl text-gray-900">
            See What Our <br />
            Happy Clients Say
          </h2>
        </div>
      </div>
      <div class="flex mb-10">
        <div class="flex items-start justify-center w-2/12">
          <img src="/testimonial-line.svg" alt="" />
        </div>
        <div class="w-8/12 mt-16">
          <h2 class="text-3xl font-light text-gray-900">
            “Funding at Bucker is very easy and comfortable. <br />
            Just need to find an idea, click and already funding.”
          </h2>
          <div class="mt-8 testimonial-info">
            <div class="text-xl font-semibold name">Shopie Nicole</div>
            <div class="text-xl font-light text-gray-400 title">
              Project Manager
            </div>
          </div>
          <div class="mt-10 testimonial-icon">
            <img
              src="/testimonial-1-icon.png"
              alt=""
              class="inline-block w-20 mr-5 rounded-full testimonial-user"
            />
            <img
              src="/testimonial-2-icon.png"
              alt=""
              class="inline-block w-20 mr-5 rounded-full testimonial-user"
            />
            <img
              src="/testimonial-3-icon.png"
              alt=""
              class="inline-block w-20 mr-5 rounded-full testimonial-user active"
            />
          </div>
        </div>
        <div class="w-2/12"></div>
      </div>
    </section>
    <div class="-mt-20 cta-clip"></div>
    <CallToAction />
    <Footer />
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const campaigns = await $axios.$get('/api/v1/campaigns')
    return { campaigns }
  },
}
</script>
