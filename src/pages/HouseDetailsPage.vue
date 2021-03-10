<template>
  <div class="house-details">
    <h1 class="text-center py-3">Welcome to the details page</h1>
    <div class="row d-flex justify-content-center">
      <div class="col-4">
    <!-- NOTE the route contains both the name and an object to provide needed params -->
    <router-link :to="{name: 'HouseDetails', params: {id: state.house._id}}" >
      <div class="card text-dark">
        <img class="card-img-top" :src="state.house.imgUrl" alt="house">
        <div class="card-body">
          <h4 class="card-title text-center"><span class="float-left">Beds: {{state.house.bedrooms}}</span> | <span class="float-right">Baths: {{state.house.bathrooms}}</span></h4>
          <p class="card-text mt-4 pt-3 text-center">Year: {{state.house.year}} | ${{state.house.price}}</p>
        </div>
      </div>
    </router-link>
  </div>
    </div>
    <button type="button" class="btn btn-outline-danger" @click="deleteHouse">
      Delete House
    </button>

    <form class="form-inline justify-content-center">
      <div class="form-group m-2">
          <input
            type="text"
            name="bedrooms"
            id="bedrooms"
            class="form-control"
            placeholder="Bedrooms"
            aria-describedby="helpId"
            v-model="state.house.bedrooms"
          />
      </div>
        <div class="form-group m-2">
          <input
            type="text"
            name="bathrooms"
            id="bathrooms"
            class="form-control"
            placeholder="Bathrooms"
            aria-describedby="helpId"
            v-model="state.house.bathrooms"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="number"
            name="year"
            id="year"
            class="form-control"
            placeholder="Year"
            aria-describedby="helpId"
            v-model="state.house.year"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="number"
            name="price"
            id="price"
            class="form-control"
            placeholder="Price"
            aria-describedby="helpId"
            v-model="state.house.price"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="text"
            name="description"
            id="description"
            class="form-control"
            placeholder="Description"
            aria-describedby="helpId"
            v-model="state.house.description"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="text"
            name="imgUrl"
            id="imgUrl"
            class="form-control"
            placeholder="ImgUrl"
            aria-describedby="helpId"
            v-model="state.house.imgUrl"
          />
        </div>
      <div>
        <button class="btn btn-info" type="submit">Create</button>
      </div>
    </form>
  </div>
</template>

<script>
import { onMounted, reactive, computed } from 'vue'
import { onBeforeRouteLeave, useRoute, useRouter } from 'vue-router'
import { housesService } from '../services/HousesService'
import { AppState } from '../Appstate'

export default {
  name: 'HouseDetails',
  setup() {
    const route = useRoute()
    const router = useRouter()
    const state = reactive({
      house: computed(() => AppState.activeHouse)
    })

    onMounted(() => {
      housesService.getHouse(route.params.id)
    })

    onBeforeRouteLeave((to, from, next) => {
      if (window.confirm('You sure bro?')) {
        AppState.activeHouse = {}
        next()
      }
    })

    return {
      route,
      state,
      async deleteHouse() {
        await housesService.deleteHouse(state.house._id)
        router.push({ name: 'Houses' })
      }
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
</style>
