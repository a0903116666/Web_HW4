<script setup>
defineProps({
  title: {
    type: String,
    required: true
  },
  subtitle: {
    type: String,
    required: true
  },
  metrics: {
    type: Array,
    required: true
  }
});
</script>

<template>
  <header class="hero panel">
    <p class="eyebrow">Vue SFC Homework</p>
    <h1>{{ title }}</h1>
    <p class="hero-copy">{{ subtitle }}</p>

    <div class="metric-grid">
      <article v-for="metric in metrics" :key="metric.label" class="metric-card">
        <span>{{ metric.label }}</span>
        <strong>{{ metric.value }}</strong>
      </article>
    </div>
  </header>
</template>

<style scoped>
.hero {
  margin-bottom: 22px;
}

.eyebrow {
  margin: 0 0 10px;
  color: var(--accent);
  letter-spacing: 0.18em;
  text-transform: uppercase;
  font-size: 0.78rem;
  font-weight: 800;
}

h1 {
  margin: 0;
  font-size: clamp(2.2rem, 4vw, 4.1rem);
  line-height: 1.02;
}

.hero-copy {
  margin: 14px 0 0;
  max-width: 68ch;
  color: var(--muted);
  line-height: 1.8;
  font-size: 1.02rem;
}

.metric-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 14px;
  margin-top: 24px;
}

.metric-card {
  padding: 16px 18px;
  border-radius: 20px;
  background: var(--accent-soft);
  border: 1px solid rgba(181, 82, 43, 0.08);
}

.metric-card span {
  display: block;
  margin-bottom: 8px;
  color: var(--muted);
  font-size: 0.82rem;
}

.metric-card strong {
  font-size: 1.05rem;
}

@media (max-width: 720px) {
  .metric-grid {
    grid-template-columns: 1fr;
  }
}
</style>