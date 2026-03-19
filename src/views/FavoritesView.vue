<template>
  <div class="favorites">
    <div class="favorites__container">
      <div class="favorites__header">
        <h1 class="favorites__title">Избранное</h1>
        <p class="favorites__subtitle" v-if="favorites.length">
          У вас {{ favorites.length }}
          {{ pluralize(favorites.length, 'объект', 'объекта', 'объектов') }} в избранном
        </p>
      </div>

      <div v-if="favorites.length" class="favorites__grid">
        <PropertyCard
          v-for="property in favorites"
          :key="property.id"
          :property="property"
          @toggle-favorite="removeFromFavorites"
        />
      </div>

      <div v-if="favorites.length" class="favorites__actions">
        <button @click="clearAllFavorites" class="favorites__clear-btn">
          🗑️ Очистить избранное
        </button>
      </div>

      <div v-else class="favorites__empty">
        <span class="favorites__empty-icon">❤️</span>
        <h2 class="favorites__empty-title">У вас пока нет избранных объектов</h2>
        <p class="favorites__empty-text">
          Добавляйте понравившиеся объекты в избранное, чтобы не потерять их
        </p>
        <router-link to="/catalog" class="favorites__empty-btn"> Перейти в каталог </router-link>
      </div>

      <div v-if="!favorites.length && recommendedProperties.length" class="favorites__recommended">
        <h2 class="favorites__recommended-title">Рекомендуем посмотреть</h2>
        <div class="favorites__recommended-grid">
          <PropertyCard
            v-for="property in recommendedProperties"
            :key="property.id"
            :property="property"
            @toggle-favorite="addToFavorites"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import PropertyCard from '@/components/PropertyCard.vue'

const favorites = ref([])
const recommendedProperties = ref([])

onMounted(() => {
  loadFavorites()
  loadRecommendedProperties()
})

const loadFavorites = () => {
  const savedFavorites = localStorage.getItem('favorites')
  if (savedFavorites) {
    try {
      favorites.value = JSON.parse(savedFavorites)
    } catch (e) {
      console.error('Ошибка загрузки избранного:', e)
      favorites.value = []
    }
  }
}

const saveFavorites = () => {
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}

watch(
  favorites,
  () => {
    saveFavorites()
  },
  { deep: true },
)

const loadRecommendedProperties = () => {
  recommendedProperties.value = [
    {
      id: 101,
      title: 'Квартира с видом на парк',
      address: 'ул. Парковая, 10',
      price: 8900000,
      area: 72,
      rooms: 3,
      type: 'apartment',
      image: '/image/home1.jpg',
      isFavorite: false,
      createdAt: '2024-02-10',
    },
    {
      id: 102,
      title: 'Дом у озера',
      address: 'пос. Озерный, 15',
      price: 18500000,
      area: 200,
      rooms: 5,
      type: 'house',
      image: '/image/home2.jpg',
      isFavorite: false,
      createdAt: '2024-02-05',
    },
    {
      id: 103,
      title: 'Студия в новостройке',
      address: 'ул. Строителей, 7',
      price: 5200000,
      area: 38,
      rooms: 1,
      type: 'apartment',
      image: '/image/home3.jpg',
      isFavorite: false,
      createdAt: '2024-02-12',
    },
  ]
}

const removeFromFavorites = (propertyId) => {
  const property = favorites.value.find((p) => p.id === propertyId)
  if (property) {
    property.isFavorite = false
    favorites.value = favorites.value.filter((p) => p.id !== propertyId)
    showNotification('Объект удален из избранного')
  }
}

const addToFavorites = (propertyId) => {
  const property = recommendedProperties.value.find((p) => p.id === propertyId)
  if (property && !favorites.value.some((p) => p.id === propertyId)) {
    property.isFavorite = true
    favorites.value.push({ ...property })
    showNotification('Объект добавлен в избранное')
  }
}

const clearAllFavorites = () => {
  if (confirm('Вы уверены, что хотите очистить весь список избранного?')) {
    favorites.value.forEach((property) => {
      property.isFavorite = false
    })
    favorites.value = []
    showNotification('Избранное очищено')
  }
}

const pluralize = (count, one, few, many) => {
  const mod10 = count % 10
  const mod100 = count % 100

  if (mod100 >= 11 && mod100 <= 19) {
    return many
  }

  if (mod10 === 1) {
    return one
  }

  if (mod10 >= 2 && mod10 <= 4) {
    return few
  }

  return many
}

const showNotification = (message) => {
  const notification = document.createElement('div')
  notification.textContent = message
  notification.style.cssText = `
    position: fixed;
    top: 20px;
    right: 20px;
    background: var(--color-primary);
    color: white;
    padding: 12px 24px;
    border-radius: var(--radius-medium);
    box-shadow: var(--shadow-medium);
    z-index: 1000;
    animation: slideIn 0.3s ease;
  `

  const style = document.createElement('style')
  style.textContent = `
    @keyframes slideIn {
      from {
        transform: translateX(100%);
        opacity: 0;
      }
      to {
        transform: translateX(0);
        opacity: 1;
      }
    }
  `
  document.head.appendChild(style)

  document.body.appendChild(notification)

  setTimeout(() => {
    notification.style.animation = 'slideIn 0.3s ease reverse'
    setTimeout(() => {
      document.body.removeChild(notification)
      document.head.removeChild(style)
    }, 300)
  }, 3000)
}
</script>

<style scoped>
.favorites {
  padding: var(--space-xxl) 0;
  min-height: 80vh;
  background-color: var(--color-background);
}

.favorites__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-l);
}

.favorites__header {
  text-align: center;
  margin-bottom: var(--space-xl);
}

.favorites__title {
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-s);
}

.favorites__subtitle {
  color: var(--color-text-light);
  font-size: 1.1rem;
}

.favorites__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: var(--space-l);
  margin-bottom: var(--space-xl);
}

.favorites__actions {
  display: flex;
  justify-content: center;
  margin-bottom: var(--space-xl);
}

.favorites__clear-btn {
  padding: var(--space-s) var(--space-l);
  background: var(--color-white);
  color: var(--color-accent);
  border: 1px solid var(--color-accent);
  border-radius: var(--radius-medium);
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.3s;
}

.favorites__clear-btn:hover {
  background: var(--color-accent);
  color: var(--color-white);
}

.favorites__empty {
  text-align: center;
  padding: var(--space-xxl);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  margin-bottom: var(--space-xl);
}

.favorites__empty-icon {
  font-size: 5rem;
  display: block;
  margin-bottom: var(--space-l);
  opacity: 0.5;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 0.5;
  }
  50% {
    transform: scale(1.1);
    opacity: 0.8;
  }
}

.favorites__empty-title {
  color: var(--color-text-dark);
  font-size: 1.8rem;
  margin-bottom: var(--space-m);
}

.favorites__empty-text {
  color: var(--color-text-light);
  font-size: 1.1rem;
  margin-bottom: var(--space-xl);
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.favorites__empty-btn {
  display: inline-block;
  padding: var(--space-m) var(--space-xl);
  background: var(--color-primary);
  color: var(--color-white);
  text-decoration: none;
  border-radius: var(--radius-medium);
  font-size: 1.1rem;
  transition: all 0.3s;
}

.favorites__empty-btn:hover {
  background: var(--color-primary-dark);
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

.favorites__recommended {
  margin-top: var(--space-xxl);
}

.favorites__recommended-title {
  text-align: center;
  font-size: 2rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-xl);
  position: relative;
}

.favorites__recommended-title::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: var(--color-primary);
  border-radius: var(--radius-small);
}

.favorites__recommended-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: var(--space-l);
}

@media (max-width: 768px) {
  .favorites__container {
    padding: 0 var(--space-m);
  }

  .favorites__title {
    font-size: 2rem;
  }

  .favorites__empty-title {
    font-size: 1.5rem;
  }

  .favorites__empty-text {
    font-size: 1rem;
  }

  .favorites__recommended-title {
    font-size: 1.5rem;
  }
}

.favorites__grid > *,
.favorites__recommended-grid > * {
  animation: fadeInUp 0.5s ease forwards;
  opacity: 0;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.favorites__grid > *:nth-child(1) {
  animation-delay: 0.1s;
}
.favorites__grid > *:nth-child(2) {
  animation-delay: 0.2s;
}
.favorites__grid > *:nth-child(3) {
  animation-delay: 0.3s;
}
.favorites__grid > *:nth-child(4) {
  animation-delay: 0.4s;
}
.favorites__grid > *:nth-child(5) {
  animation-delay: 0.5s;
}
.favorites__grid > *:nth-child(6) {
  animation-delay: 0.6s;
}
</style>
