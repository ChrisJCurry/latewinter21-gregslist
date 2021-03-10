<template>
  <div class="home container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1 class="text-white">Welcome to Gregslist!</h1>
        <h1 class="text-white">Please select your collection to look at</h1>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col text-center">
      <router-link :to="{name: 'Cars'}">
        <button class="btn btn-primary btn-lg mx-3">Cars</button>
      </router-link>
      <router-link :to="{name: 'Houses'}">
        <button class="btn btn-primary btn-lg mx-3">Houses</button>
      </router-link>
      <button class="btn btn-primary btn-lg mx-3">Jobs</button>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue'

export default {
  name: 'Home',
  setup() {
    const state = reactive({})
    return {
      methodName() {

      }
    }
  },
  components: {
  }
}
</script>
