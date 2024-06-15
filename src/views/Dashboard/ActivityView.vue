<script setup lang="ts">
import DataStatsActivity from '@/components/Dashboard/DataStatsActivity.vue';
import ActivityChart from '@/components/Dashboard/ActivityChart.vue'
import { getDailyActivities } from '@/services/activities';
import { useLoadingStore } from '@/stores/loading';

const loadingStore = useLoadingStore();
onBeforeMount(async () => {
  try {
  } catch (error) {
    
  }
});
const activities = ref<Activity[]>([])
interface Activity {
  _id: string;
  user: string;
  report_date: string;
  web_activities: Webactivity[];
  createdAt: string;
  updatedAt: string;
  __v: number;
}

interface Webactivity {
  _id: string;
  domain: string;
  time_limit: number;
  time_start: number;
  time_total: number;
  page_views: number;
  user: string;
  is_blocked: boolean;
  createdAt: string;
  updatedAt: string;
  __v: number;
}

const chartActivities = computed(() => {
  const seriesDomain: string[] = []
  activities.value.forEach((activity) => {
    activity.web_activities.forEach((webActivity) => {
      if (!seriesDomain.includes(webActivity.domain)) {
        seriesDomain.push(webActivity.domain)
      }
    })
  })
  const activitiesData = seriesDomain.map((domain) => {
    const data = activities.value.map((activity) => {
      const webActivity = activity.web_activities.find((webActivity) => webActivity.domain === domain)
      return webActivity ? webActivity.time_total : 0
    })
    return {
      name: domain,
      data
    }
  })
  console.log(activitiesData)
  return {
    series: activitiesData,
    labels: activities.value.map((activity) => activity.createdAt.split('T')[0])
  }
})
</script>

<template>
    <div class="grid grid-cols-1 gap-4 md:grid-cols-2 md:gap-6 xl:grid-cols-4 2xl:gap-7.5">
      <DataStatsActivity v-if="chartActivities" :chart-activities="chartActivities"/>
    </div>

    <div class="mt-4  gap-4 md:mt-6 md:gap-6 2xl:mt-7.5 2xl:gap-7.5">
      <!-- ====== Chart One Start -->
      <!-- ====== Chart One End -->

      <!-- ====== Chart Two Start -->
      <ActivityChart :chart-activities="chartActivities"/>
      <!-- ====== Chart Two End -->

      <!-- ====== Chart Three Start -->
      <!-- <ChartThree /> -->
      <!-- ====== Chart Three End -->

      <!-- ====== Map One Start -->
      <!-- ====== Map One End -->

      <!-- ====== Table One Start -->
      <div class="col-span-12 xl:col-span-8">
      </div>
      <!-- ====== Table One End -->

      <!-- ====== Chat Card Start -->
      <!-- ====== Chat Card End -->
    </div>
</template>
