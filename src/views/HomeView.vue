<script setup lang="ts">
import BaseApi from '@/api-requests/api'
import type { IPage, JobListing, PositionFunction } from '@/models/models'
import { inject, onMounted, ref } from 'vue'
import type { AxiosStatic } from 'axios'
import JobFeed from '../components/JobFeed.vue'

const mojobApi = ref<BaseApi | null>(null)
const jobListingFilters = ref<JobListing[]>([])
const positionFunctionFilters = ref<PositionFunction[]>([])
const axiosInstance = inject('axios') as AxiosStatic

onMounted(async () => {
  mojobApi.value = new BaseApi('https://test-api.mojob.io/public/', axiosInstance)
  try {
    const jobListingsResponsePage: IPage<JobListing> = await mojobApi.value.getJobListings();
    if (jobListingsResponsePage.results) {
      jobListingFilters.value = jobListingsResponsePage.results;
    } else {
      console.log('Failed loading job listing function filters')
    }
    
    const jobLocationFiltersResponsePage: IPage<PositionFunction> =
      await mojobApi.value.getPositionFunctions()
    if (jobLocationFiltersResponsePage.results) {
      positionFunctionFilters.value = jobLocationFiltersResponsePage.results
      // console.log(JSON.stringify(positionFunctionFilters.value, null, 2))
      console.log(positionFunctionFilters.value)
    } else {
      console.log('Failed loading position function filters')
    }
  } catch (e) {
    console.log('Failed loading position function filters')
    console.log(e)
  }
})
</script>

<template>
  <div class="home">
    <job-feed :job-listings="jobListingFilters" :position-functions="positionFunctionFilters" />
  </div>
</template>
