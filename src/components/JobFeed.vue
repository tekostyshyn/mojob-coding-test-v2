<script setup lang="ts">
import type { JobListing, PositionFunction } from '@/models/models'

export interface Props {
  jobListings: JobListing[]
  positionFunctions: PositionFunction[]
}
const props = withDefaults(defineProps<Props>(), {
  jobListings: () => [],
  positionFunctions: () => []
});
</script>

<template>
  <div class="container">
    <ul class="job-feed">
      <li class="job-item" v-for="{ job } in props.jobListings" :key="job?.id">
        <p class="job-title">{{ job?.title }}</p>
        <div class="job-description-wrapper">
          <p class="job-description">{{ job?.unit?.name }}</p>
          <p class="job-description dot">{{ job?.position_function?.name_nb }} </p>
          <p class="job-description dot">{{ job && job.due_date && new Date(job.due_date).toDateString() }}</p>

        </div>
      </li>
    </ul>
  </div>
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
  font-size: 26px;
  font-weight: 600;
  margin-bottom: 10px;
}

.dot::before {
  display: inline-block;
  content: "";
  background-color: darkgrey;
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.job-description-wrapper {
  display: flex;
  gap: 10px;
}

.job-description {
  display: flex;
  gap: 10px;
  align-items: center;
  font-size: 20px;
}
</style>