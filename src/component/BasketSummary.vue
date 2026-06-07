<script setup>
const props = defineProps({
  summary: {
    type: Object,
    required: true
  },
  filters: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['update-filters']);

function updateKeyword(event) {
  emit('update-filters', {
    ...props.filters,
    keyword: event.target.value
  });
}
</script>

<template>
  <section class="panel">
    <h2 class="section-title">簡易查詢</h2>
    <p class="section-copy">輸入日期、商品名稱或價格即可縮小歷史資料範圍。</p>

    <div class="filter-grid">
      <label>
        <span>搜尋關鍵字</span>
        <input :value="filters.keyword" type="search" placeholder="搜尋日期、商品名稱或價格" @input="updateKeyword" />
      </label>
    </div>

    <div class="summary-grid">
      <article class="summary-card">
        <span>最後一筆日期</span>
        <strong>{{ summary.latest?.recordDate || '尚無資料' }}</strong>
      </article>
      <article class="summary-card">
        <span>最後一筆商品</span>
        <strong>{{ summary.latest?.productName || '---' }}</strong>
      </article>
      <article class="summary-card">
        <span>最後一筆價格</span>
        <strong>{{ summary.latest ? `$${summary.latest.productPrice}` : '$0' }}</strong>
      </article>
    </div>
  </section>
</template>

<style scoped>
.filter-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
  margin-top: 18px;
}

label {
  display: grid;
  gap: 8px;
}

span {
  font-size: 0.92rem;
  font-weight: 700;
  color: var(--accent-dark);
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 12px;
  margin-top: 18px;
}

.summary-card {
  border-radius: 20px;
  padding: 16px 18px;
  background: rgba(253, 232, 220, 0.92);
  border: 1px solid rgba(181, 82, 43, 0.08);
}

.summary-card span {
  display: block;
  margin-bottom: 10px;
  color: var(--muted);
  font-size: 0.82rem;
}

.summary-card strong {
  display: block;
  font-size: 1.05rem;
}

@media (max-width: 720px) {
  .filter-grid,
  .summary-grid {
    grid-template-columns: 1fr;
  }
}
</style>