<template>
  <div class="container flex items-center justify-center h-screen mx-auto">
    <div class="w-full px-10 lg:w-1/3 lg:px-0">
      <div class="flex items-center justify-center px-12 mx-auto mt-6 mb-8">
        <img src="/404-illustration.svg" alt="" class="w-full" />
      </div>
      <h2 class="mb-3 text-3xl font-medium text-center">Oops. Something went wrong!</h2>
      <p class="font-light text-center">
        An error occured when processing your transaction
        <br>
        Please try again later
      </p>
      <div class="mx-10 mt-6 mb-4">
        <div class="mb-3">
          <button
            @click="$router.push({ path: '/' })"
            class="block w-full px-6 py-4 text-lg font-semibold text-white rounded-full bg-orange-button hover:bg-green-button"
          >
            Fund Other Project
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'success',
}
</script>
