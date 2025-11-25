<!-- CatalogView.vue -->
<template>
  <div class="catalog">
    <div class="catalog__container container">
      <h1 class="catalog__title">Каталог недвижимости</h1>
      <p class="catalog__description">Скоро здесь будет полный каталог объектов</p>
    </div>
  </div>
</template>

<script setup></script>

<style scoped>
.catalog {
  padding: var(--space-xxl) 0;
}

.catalog__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.catalog__title {
  color: var(--color-text-dark);
  margin-bottom: var(--space-m);
  font-size: 2.5rem;
}

.catalog__description {
  color: var(--color-text-light);
  font-size: 1.1rem;
}
</style>
