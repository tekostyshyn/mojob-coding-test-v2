<script setup lang="ts">
import type { JobListing, PositionFunction } from '@/models/models'
import { computed, ref, watch } from 'vue';

export interface Props {
  jobListings: JobListing[]
  positionFunctions: PositionFunction[]
}
const props = withDefaults(defineProps<Props>(), {
  jobListings: () => [],
  positionFunctions: () => []
});

const emit = defineEmits<{
  (e: 'handleAmount', value: string): void
}>()

const showFilterBtn = ref<boolean>(false);
const showPagesBtn = ref<boolean>(false);
const checkedPositions = ref<string[]>([]);
const jobsPerPage = ref<string>('');

watch(jobsPerPage, () => {
  emit("handleAmount", jobsPerPage.value);
})

function toggleBtn() {
  showFilterBtn.value = !showFilterBtn.value;
}

function togglePagesBtn() {
  showPagesBtn.value = !showPagesBtn.value
}

const setBtnClasses = computed(function () {
  return showFilterBtn.value ? 'filter-list' : 'visually-hidden'
})

const setPagesBtnClasses = computed(function () {
  return showPagesBtn.value ? 'pages-list' : 'visually-hidden'
})

const filterFunc = computed(function () {
  if (checkedPositions.value.length === 0) {
    return props.jobListings;
  }

  return props.jobListings.filter(item => {
    const index = checkedPositions.value.findIndex(position => position === item.job?.position_function?.name_nb)
    return index >= 0 ? true : false;
  })
})

const setPagesButtonText = computed(function () {
  if (jobsPerPage.value === '25') {
    return '25 per page'
  } else if (jobsPerPage.value === '200') {
    return 'Display all'
  }
  return '5 per page'
})

</script>


<template>
  <div class="container">
    <div class="wrapper">
      <div class="filter">
        <button class="filter-button" @click="toggleBtn">Filter by position</button>
        <div :class="setBtnClasses">
          <div v-for="position in positionFunctions" :key="position.id" class="filter-box">
            <label v-if="position.children?.length === 1">
              <input type="checkbox" :name="position.name_nb" :value="position.name_nb" v-model="checkedPositions">
              {{ position.name_nb }}
            </label>
            <div v-else class="filter-box">
              <p>{{ position.name_nb }}</p>
              <label v-for="subposition in position.children" :key="subposition.id" class="filter-box-label">
              <input type="checkbox" :name="subposition.name_nb" :value="subposition.name_nb" v-model="checkedPositions">
              {{ subposition.name_nb }}
            </label>
            </div>
          </div>
        </div>
      </div>
      <div class="pages">
        <button class="filter-button" @click="togglePagesBtn">{{ setPagesButtonText }}</button>
        <div :class="setPagesBtnClasses">
          <label>
            <input type="radio" value="5" v-model="jobsPerPage">
            5 per page
          </label>
          <label>
            <input type="radio" value="25" v-model="jobsPerPage">
            25 per page
          </label>
          <label>
            <input type="radio" value="200" v-model="jobsPerPage">
            Display all
          </label>
        </div>
      </div>
    </div>
    <ul class="job-feed">
      <li class="job-item" v-for="{ job } in filterFunc" :key="job?.id">
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
  max-width: 1000px;
  margin-left: auto;
  margin-right: auto;
}

.wrapper {
  display: flex;
  justify-content: space-between;
}

.filter {
  position: relative;

}

.filter-box {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: flex-start;
}

.filter-box-label {
  padding-left: 20px;
}

.filter-button {
  display: block;
  height: 60px;
  width: 240px;
  font-size: 18px;
  margin-bottom: 10px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  border: none;
  cursor: pointer;
  text-transform: uppercase;
}


.filter-list {
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 10px;
  height: 400px;
  width: 500px;
  overflow-y: scroll;
  font-size: 20px;
  z-index: 2;
  background-color: white;
  padding: 15px;
  border: 1px solid darkgray;
}

.pages-list {
  box-sizing: border-box;
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 10px;
  overflow-y: scroll;
  font-size: 20px;
  width: 200px;
  z-index: 2;
  background-color: white;
  padding: 15px;
  border: 1px solid darkgray;
}

.job-feed {
  list-style: none;
  z-index: 1;
}

.job-item {
  position: relative;
  display: block;
  text-align: left;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  ;
  margin-bottom: 20px;
  padding: 20px;
  padding-left: 70px;
}

.job-item::before {
  position: absolute;
  display: inline-block;
  content: "";
  background-color: lightgrey;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
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

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  border: 0;
  padding: 0;

  white-space: nowrap;
  clip-path: inset(100%);
  clip: rect(0 0 0 0);
  overflow: hidden;
}
</style>