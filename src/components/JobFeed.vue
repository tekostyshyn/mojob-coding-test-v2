<script setup lang="ts">
import type { JobListing, PositionFunction } from '@/models/models'
import { computed } from 'vue'

export interface Props {
  jobListings: JobListing[]
  positionFunctions: PositionFunction[]
}
const props = withDefaults(defineProps<Props>(), {
  jobListings: () => [],
  positionFunctions: () => []
})

console.log(props.jobListings);
// const titles = computed(() => (props.jobListings.map(res => res.job)));
// console.log(titles);



</script>

<template>
  <div class="container">
    <ul class="job-feed">
      <li class="job-item" v-for="{ job } in props.jobListings" :key="job?.id">
        <p class="job-title">{{ job?.title }}</p>
        <p class="job-description">{{ job?.unit?.name }}</p>
        <p class="job-description">{{ job?.position_function?.name_nb }}</p>
        <p class="job-description">{{ job?.due_date }}</p>
      </li>
    </ul>
  </div>
  <!-- <ul>
      <li v-for="item in props.jobListings" :key={{ item.job.key }}>
        {{ item }}
      </li>
    </ul> -->
</template>

<style>
.container {
  padding: 20px 60px;
}

.job-feed {
  list-style: none;
}

.job-item {
  display: block;
  text-align: left;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  ;
  margin-bottom: 20px;
  padding: 20px;
}

.job-title {
  display: block;
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 10px;
  color: black;
}

.job-description {
  color: darkgrey;
  font-size: 20px;
  display: inline-block;
}
</style>