<template>
  <div class="job-details">
    <h1 class="text-center py-3">Welcome to the details page</h1>
    <div class="row d-flex justify-content-center">
      <div class="col-4">
    <!-- NOTE the route contains both the name and an object to provide needed params -->
    <router-link :to="{name: 'JobDetails', params: {id: state.job._id}}" >
      <div class="card text-dark">
        <div class="card-body">
          <h4 class="card-title text-center"><span class="float-left">{{state.job.jobTitle}}</span> | <span class="float-right">{{state.job.company}}</span></h4>
          <p class="card-text mt-4 pt-3 text-center">{{state.job.hours}} | ${{state.job.rate}}</p>
        </div>
      </div>
    </router-link>
  </div>
    </div>
    <button type="button" class="btn btn-outline-danger" @click="deleteJob">
      Delete Job
    </button>

    <form class="form-inline justify-content-center">
      <div class="form-group m-2">
          <input
            type="text"
            name="jobTitle"
            id="jobTitle"
            class="form-control"
            placeholder="Job Title"
            aria-describedby="helpId"
            v-model="state.job.jobTitle"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="text"
            name="company"
            id="company"
            class="form-control"
            placeholder="Company"
            aria-describedby="helpId"
            v-model="state.job.company"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="number"
            name="hours"
            id="hours"
            class="form-control"
            placeholder="Hours"
            aria-describedby="helpId"
            v-model="state.job.hours"
          />
        </div>
        <div class="form-group m-2">
          <input
            type="number"
            name="rate"
            id="rate"
            class="form-control"
            placeholder="Rate"
            aria-describedby="helpId"
            v-model="state.job.rate"
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
            v-model="state.job.description"
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
import { jobsService } from '../services/JobsService'
import { AppState } from '../Appstate'

export default {
  name: 'JobDetails',
  setup() {
    const route = useRoute()
    const router = useRouter()
    const state = reactive({
      job: computed(() => AppState.activeJob)
    })

    onMounted(() => {
      jobsService.getJob(route.params.id)
    })

    onBeforeRouteLeave((to, from, next) => {
      if (window.confirm('You sure bro?')) {
        AppState.activeJob = {}
        next()
      }
    })

    return {
      route,
      state,
      async deleteJob() {
        await jobsService.deleteJob(state.job._id)
        router.push({ name: 'Jobs' })
      }
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
</style>
