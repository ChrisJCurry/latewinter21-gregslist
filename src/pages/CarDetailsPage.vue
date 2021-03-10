<template>
  <div class="car-details">
    <h1 class="text-center py-3">Welcome to the details page</h1>
    <div class="row d-flex justify-content-center">
      <div class="col-4">
    <!-- NOTE the route contains both the name and an object to provide needed params -->
    <router-link :to="{name: 'CarDetails', params: {id: state.car._id}}" >
      <div class="card text-dark">
        <img class="card-img-top" :src="state.car.imgUrl" alt="car">
        <div class="card-body">
          <h4 class="card-title text-center"><span class="float-left">{{state.car.make}}</span> | <span class="float-right">{{state.car.model}}</span></h4>
          <p class="card-text mt-4 pt-3 text-center">Year: {{state.car.year}} | ${{state.car.price}}</p>
        </div>
      </div>
    </router-link>
  </div>
    </div>
    <button type="button" class="btn btn-outline-danger" @click="deleteCar">
      Delete Car
    </button>

    <form class="form-inline justify-content-center">
      <div class="form-group m-2">
        <input
          type="text"
          name="make"
          id="make"
          class="form-control"
          placeholder="Make"
          aria-describedby="helpId"
          v-model="state.car.make"
        />
      </div>
      <div class="form-group m-2">
        <input
          type="text"
          name="model"
          id="model"
          class="form-control"
          placeholder="Model"
          aria-describedby="helpId"
          v-model="state.car.model"
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
          v-model="state.car.year"
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
          v-model="state.car.price"
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
          v-model="state.car.description"
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
          v-model="state.car.imgUrl"
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
import { carsService } from '../services/CarsService'
import { AppState } from '../Appstate'

export default {
  name: 'CarDetails',
  setup() {
    const route = useRoute()
    const router = useRouter()
    const state = reactive({
      car: computed(() => AppState.activeCar)
    })

    onMounted(() => {
      carsService.getCar(route.params.id)
    })

    onBeforeRouteLeave((to, from, next) => {
      if (window.confirm('You sure bro?')) {
        AppState.activeCar = {}
        next()
      }
    })

    return {
      route,
      state,
      async deleteCar() {
        await carsService.deleteCar(state.car._id)
        router.push({ name: 'Cars' })
      }
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
</style>
