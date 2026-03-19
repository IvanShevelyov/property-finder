<!-- PropertyCard.vue -->
<template>
  <div class="property-card">
    <div class="property-card__image">
      <img :src="property.image" :alt="property.title" />
      <button
        class="property-card__favorite"
        :class="{ 'property-card__favorite--active': property.isFavorite }"
        @click="$emit('toggle-favorite', property.id)"
      >
        ❤
      </button>
    </div>

    <div class="property-card__content">
      <h3 class="property-card__title">{{ property.title }}</h3>
      <p class="property-card__address">{{ property.address }}</p>

      <div class="property-card__details">
        <span class="property-card__detail">
          <span class="property-card__detail-icon">📐</span>
          {{ property.area }} м²
        </span>
        <span class="property-card__detail">
          <span class="property-card__detail-icon">🛏️</span>
          {{ property.rooms }} комн.
        </span>
      </div>

      <div class="property-card__price">{{ formatPrice(property.price) }} ₽</div>

      <button class="property-card__btn">Подробнее</button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  property: {
    type: Object,
    required: true,
  },
})

defineEmits(['toggle-favorite'])

const formatPrice = (price) => {
  return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}
</script>

<style scoped>
.property-card {
  background: var(--color-white);
  border-radius: var(--radius-large);
  overflow: hidden;
  box-shadow: var(--shadow-light);
  transition: all 0.3s;
}

.property-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-medium);
}

.property-card__image {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.property-card__image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s;
}

.property-card:hover .property-card__image img {
  transform: scale(1.05);
}

.property-card__favorite {
  position: absolute;
  top: var(--space-m);
  right: var(--space-m);
  width: 40px;
  height: 40px;
  background: var(--color-white);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  color: var(--color-text-light);
  transition: all 0.3s;
  box-shadow: var(--shadow-light);
}

.property-card__favorite:hover {
  transform: scale(1.1);
}

.property-card__favorite--active {
  color: var(--color-accent);
}

.property-card__content {
  padding: var(--space-l);
}

.property-card__title {
  color: var(--color-text-dark);
  font-size: 1.2rem;
  margin-bottom: var(--space-xs);
}

.property-card__address {
  color: var(--color-text-light);
  font-size: 0.9rem;
  margin-bottom: var(--space-m);
}

.property-card__details {
  display: flex;
  gap: var(--space-m);
  margin-bottom: var(--space-m);
}

.property-card__detail {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  color: var(--color-text-light);
  font-size: 0.9rem;
}

.property-card__detail-icon {
  font-size: 1.1rem;
}

.property-card__price {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--color-primary);
  margin-bottom: var(--space-m);
}

.property-card__btn {
  width: 100%;
  padding: var(--space-s);
  background: var(--color-primary);
  color: var(--color-white);
  border: none;
  border-radius: var(--radius-medium);
  cursor: pointer;
  transition: all 0.3s;
  font-size: 1rem;
}

.property-card__btn:hover {
  background: var(--color-primary-dark);
}
</style>
