<template>
  <div class=""></div>
  <div class="statistics-page">
    <StatisticsFilterBox @updateFilter="onFilterUpdate" />
    <StatisticsGraph
      :categoryRatios="categoryRatios"
      :filteredData="filteredData"
      :selectedPeriod="selectedPeriod"
      :monthlyTotal="monthlyTotal"
      :dailyTotal="dailyTotal"
    />

    <div class="category__ratio p-4">
      <!-- <p>🔹 전체 합계: {{ store.totalAmount }} 원</p> -->
      <CategoryList :categoryRatios="categoryRatios" />
      <!-- <ul>
            <li v-for="item in store.categoryRatios" :key="item.category">
              {{ item.category }}: {{ item.amount }}원 ({{ item.ratio }}%)
            </li>
          </ul> -->
    </div>
    <div class="m-margin"></div>
  </div>
</template>

<script setup>
import '@/css/statistics/statistics.css';
import StatisticsFilterBox from './StatisticsFilterBox.vue';
import StatisticsGraph from './StatisticsGraph.vue';
import CategoryList from './CategoryList.vue';
import { useStatisticsStore } from '@/stores/useStatisticsStore';
import { onMounted } from 'vue';
import { storeToRefs } from 'pinia';
import { useCategoryStore } from '@/stores/category';

const store = useStatisticsStore();

const {
  selectedType,
  selectedPeriod,
  selectedYear,
  selectedMonth,
  categoryRatios,
  filteredData,
  monthlyTotal,
  dailyTotal,
} = storeToRefs(store);

onMounted(() => {
  store.fetchData();
  useCategoryStore().fetchcategoryList();
});

const onFilterUpdate = ({ type, period, year, month }) => {
  console.log('🔄 필터 업데이트:', type, period, year, month);

  store.selectedType = type;
  store.selectedPeriod = period;
  store.selectedYear = year;
  store.selectedMonth = month;
};
</script>

<style scoped>
.m-margin {
  margin: 7rem;
  display: none;
}
@media (max-width: 500px) {
  .m-margin {
    display: block;
  }
}
</style>
