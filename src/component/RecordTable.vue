<script setup>
defineProps({
  records: {
    type: Array,
    required: true
  }
});
</script>

<template>
  <section class="panel">
    <h2 class="section-title">資料明細</h2>
    <p class="section-copy">把價格歷史整理成表格，方便比對每一筆紀錄。</p>

    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th>日期</th>
            <th>商品名稱</th>
            <th>商品價格</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="record in records" :key="record.id">
            <td>{{ record.recordDate }}</td>
            <td>{{ record.productName }}</td>
            <td>${{ record.productPrice }}</td>
          </tr>
          <tr v-if="!records.length" class="empty-row">
            <td colspan="3">目前沒有符合條件的資料。</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<style scoped>
.table-wrap {
  margin-top: 18px;
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 14px 14px;
  text-align: left;
  border-bottom: 1px solid rgba(109, 88, 70, 0.12);
}

th {
  color: var(--accent-dark);
  font-size: 0.9rem;
}

tbody tr:hover {
  background: rgba(253, 232, 220, 0.72);
}

.empty-row td {
  padding: 24px 14px;
  text-align: center;
  color: var(--muted);
}
</style>