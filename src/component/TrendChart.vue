<script setup>
import { computed } from 'vue';

const props = defineProps({
  records: {
    type: Array,
    required: true
  }
});

const chartData = computed(() => {
  const width = 720;
  const height = 280;
  const padding = 28;
  const chartWidth = width - padding * 2;
  const chartHeight = height - padding * 2;
  const values = props.records.map((record) => record.productPrice);

  if (!values.length) {
    return { width, height, points: [], path: '' };
  }

  const max = Math.max(...values);
  const min = Math.min(...values);
  const range = Math.max(max - min, 1);
  const step = props.records.length > 1 ? chartWidth / (props.records.length - 1) : 0;

  const sortedRecords = props.records.slice().sort((a, b) => new Date(a.recordDate) - new Date(b.recordDate));

  const points = sortedRecords.map((record, index) => {
    const x = padding + index * step;
    const y = padding + chartHeight - ((record.productPrice - min) / range) * chartHeight;
    return { x, y, record };
  });

  return {
    width,
    height,
    points,
    path: points.map((point) => `${point.x},${point.y}`).join(' ')
  };
});
</script>

<template>
  <section class="panel chart-panel">
    <h2 class="section-title">價格趨勢圖</h2>
    <p class="section-copy">視覺化呈現維力炸醬麵價格隨時間的變化。</p>

    <div v-if="records.length" class="chart-shell">
      <svg :viewBox="`0 0 ${chartData.width} ${chartData.height}`" role="img" aria-label="物價趨勢圖">
        <polyline :points="chartData.path" class="chart-line" />
        <circle
          v-for="point in chartData.points"
          :key="point.record.id"
          :cx="point.x"
          :cy="point.y"
          r="6"
          class="chart-point"
        />
      </svg>

      <ul class="chart-legend">
        <li v-for="record in records" :key="record.id">
          <span>{{ record.recordDate }}</span>
          <strong>{{ record.productName }}</strong>
          <em>${{ record.productPrice }}</em>
        </li>
      </ul>
    </div>

    <p v-else class="empty-state">沒有符合條件的資料，請先調整查詢關鍵字。</p>
  </section>
</template>

<style scoped>
.chart-panel {
  min-height: 100%;
}

.chart-shell {
  display: grid;
  gap: 16px;
  margin-top: 16px;
}

svg {
  width: 100%;
  height: auto;
  border-radius: 22px;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.9), rgba(253, 232, 220, 0.9));
  border: 1px solid rgba(181, 82, 43, 0.08);
}

.chart-line {
  fill: none;
  stroke: var(--accent);
  stroke-width: 4;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.chart-point {
  fill: var(--accent);
  stroke: #fff;
  stroke-width: 3;
}

.chart-legend {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
  gap: 12px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.chart-legend li {
  padding: 14px 16px;
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.72);
  border: 1px solid rgba(181, 82, 43, 0.08);
}

.chart-legend span,
.chart-legend em {
  display: block;
  font-style: normal;
  color: var(--muted);
  font-size: 0.88rem;
}

.chart-legend strong {
  display: block;
  margin: 6px 0 8px;
}

.empty-state {
  margin-top: 18px;
  color: var(--muted);
}
</style>