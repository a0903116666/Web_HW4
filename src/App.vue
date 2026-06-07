<script setup>
import { computed, ref, watch } from 'vue';
import HeroBanner from './component/HeroBanner.vue';
import InflationForm from './component/InflationForm.vue';
import BasketSummary from './component/BasketSummary.vue';
import TrendChart from './component/TrendChart.vue';
import RecordTable from './component/RecordTable.vue';

const storageKey = 'vue-sfc-price-records';

const initialRecords = [
  { id: 1, recordDate: '2026-04-01', productName: '維力炸醬麵', productPrice: 12 },
  { id: 2, recordDate: '2026-04-15', productName: '維力炸醬麵', productPrice: 13 },
  { id: 3, recordDate: '2026-05-01', productName: '維力炸醬麵', productPrice: 14 }
];

function loadRecords() {
  if (typeof window === 'undefined') {
    return initialRecords;
  }

  try {
    const savedRecords = window.localStorage.getItem(storageKey);

    if (!savedRecords) {
      return initialRecords;
    }

    const parsedRecords = JSON.parse(savedRecords);

    return Array.isArray(parsedRecords) && parsedRecords.length ? parsedRecords : initialRecords;
  } catch {
    return initialRecords;
  }
}

function saveRecords(nextRecords) {
  if (typeof window === 'undefined') {
    return;
  }

  window.localStorage.setItem(storageKey, JSON.stringify(nextRecords));
}

const records = ref(loadRecords());

const filters = ref({
  category: '全部',
  keyword: ''
});

const nextId = computed(() => records.value.length + 1);

const filteredRecords = computed(() => {
  const keyword = filters.value.keyword.trim().toLowerCase();

  return records.value.filter((record) => {
    const matchesKeyword = !keyword || [record.recordDate, record.productName, String(record.productPrice)]
      .some((value) => value.toLowerCase().includes(keyword));

    return matchesKeyword;
  });
});

const summary = computed(() => {
  const prices = records.value.map((record) => record.productPrice);
  const latest = records.value[records.value.length - 1];

  return {
    averagePrice: prices.length ? prices.reduce((total, price) => total + price, 0) / prices.length : 0,
    highestPrice: prices.length ? Math.max(...prices) : 0,
    latest
  };
});

function addRecord(payload) {
  records.value = [
    ...records.value,
    {
      id: nextId.value,
      recordDate: payload.recordDate,
      productName: payload.productName,
      productPrice: Number(payload.productPrice)
    }
  ];
}

function updateFilters(payload) {
  filters.value = payload;
}

watch(records, (nextRecords) => {
  saveRecords(nextRecords);
}, { deep: true });
</script>

<template>
  <div class="page-shell">
    <HeroBanner
      title="維力炸醬麵價格追蹤"
      subtitle="用 Vue 單頁元件整理維力炸醬麵的價格變化，把表單、查詢、圖表與資料表拆成容易維護的區塊。"
      :metrics="[
        { label: '觀測筆數', value: `${records.length} 筆` },
        { label: '平均價格', value: `$${summary.averagePrice.toFixed(0)}` },
        { label: '最高價格', value: `$${summary.highestPrice}` }
      ]"
    />

    <main class="layout">
      <section class="stack">
        <InflationForm @submit-record="addRecord" />
        <BasketSummary
          :summary="summary"
          :filters="filters"
          @update-filters="updateFilters"
        />
      </section>

      <section class="stack">
        <TrendChart :records="filteredRecords" />
        <RecordTable :records="filteredRecords" />
      </section>
    </main>
  </div>
</template>