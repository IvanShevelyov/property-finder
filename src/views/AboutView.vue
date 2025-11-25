<template>
  <div class="about">
    <div class="about__container">
      <h1 class="about__title">О компании</h1>
      <p class="about__description">
        PropertyFinder - современная платформа для поиска недвижимости
      </p>
    </div>
  </div>
</template>

<script setup>
// Логика страницы "О компании" будет добавляться
</script>

<style scoped>
.about {
  padding: var(--space-xxl) 0;
}

.about__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.about__title {
  color: var(--color-text-dark);
  margin-bottom: var(--space-m);
  font-size: 2.5rem;
}

.about__description {
  color: var(--color-text-light);
  font-size: 1.1rem;
}
</style>
