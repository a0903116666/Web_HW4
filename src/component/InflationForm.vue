<script setup>
import { reactive } from 'vue';

const emit = defineEmits(['submit-record']);

const form = reactive({
  recordDate: '2026-05-01',
  productName: '維力炸醬麵',
  productPrice: 14
});

function submitForm() {
  emit('submit-record', { ...form });
}
</script>

<template>
  <section class="panel">
    <h2 class="section-title">新增價格紀錄</h2>
    <p class="section-copy">輸入日期、商品名稱與價格，資料會儲存在瀏覽器 localStorage 中，重新整理後仍會保留。</p>

    <form class="form-grid" @submit.prevent="submitForm">
      <label>
        <span>日期</span>
        <input v-model="form.recordDate" type="date" required />
      </label>

      <label>
        <span>商品名稱</span>
        <input v-model="form.productName" type="text" placeholder="例如：維力炸醬麵" required />
      </label>

      <label>
        <span>商品價格</span>
        <input v-model.number="form.productPrice" type="number" min="0" step="1" placeholder="例如：14" required />
      </label>

      <button class="primary-button" type="submit">加入紀錄</button>
    </form>
  </section>
</template>

<style scoped>
.form-grid {
  display: grid;
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

.primary-button {
  margin-top: 4px;
  padding: 13px 16px;
  border-radius: 16px;
  color: #fff;
  background: linear-gradient(135deg, var(--accent), #d06c33);
  box-shadow: 0 14px 26px rgba(181, 82, 43, 0.24);
}
</style>