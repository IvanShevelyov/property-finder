<template>
  <div class="favorites">
    <div class="favorites__container">
      <h1 class="favorites__title">Избранное</h1>
      <p class="favorites__description">Скоро здесь будут ваши избранные объекты</p>
    </div>
  </div>
</template>

<script setup>
// Логика избранного будет добавляться
</script>

<style scoped>
.favorites {
  padding: var(--space-xxl) 0;
}

.favorites__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.favorites__title {
  color: var(--color-text-dark);
  margin-bottom: var(--space-m);
  font-size: 2.5rem;
}

.favorites__description {
  color: var(--color-text-light);
  font-size: 1.1rem;
}
</style>
