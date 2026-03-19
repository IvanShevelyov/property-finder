<template>
  <div class="catalog">
    <div class="catalog__container">
      <div class="catalog__header">
        <h1 class="catalog__title">Каталог недвижимости</h1>
        <p class="catalog__subtitle">
          Найдите идеальное жилье среди {{ filteredProperties.length }} объектов
        </p>
      </div>

      <div class="catalog__filters">
        <div class="catalog__filters-row">
          <div class="catalog__filter-group catalog__filter-group--search">
            <label class="catalog__filter-label">Поиск</label>
            <input
              type="text"
              v-model="filters.search"
              class="catalog__filter-input"
              placeholder="Адрес или название"
            />
          </div>

          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Тип</label>
            <select v-model="filters.type" class="catalog__filter-select">
              <option value="">Все типы</option>
              <option value="apartment">Квартира</option>
              <option value="house">Дом</option>
              <option value="cottage">Коттедж</option>
              <option value="commercial">Коммерческая</option>
            </select>
          </div>

          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Комнаты</label>
            <select v-model="filters.rooms" class="catalog__filter-select">
              <option value="">Любое</option>
              <option value="1">1 комната</option>
              <option value="2">2 комнаты</option>
              <option value="3">3 комнаты</option>
              <option value="4">4+ комнат</option>
            </select>
          </div>
        </div>

        <div class="catalog__filters-row">
          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Цена от</label>
            <input
              type="number"
              v-model.number="filters.priceMin"
              class="catalog__filter-input"
              placeholder="От"
            />
          </div>

          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Цена до</label>
            <input
              type="number"
              v-model.number="filters.priceMax"
              class="catalog__filter-input"
              placeholder="До"
            />
          </div>

          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Площадь от</label>
            <input
              type="number"
              v-model.number="filters.areaMin"
              class="catalog__filter-input"
              placeholder="м²"
            />
          </div>

          <div class="catalog__filter-group">
            <label class="catalog__filter-label">Сортировка</label>
            <select v-model="sortBy" class="catalog__filter-select">
              <option value="price-asc">Цена (по возрастанию)</option>
              <option value="price-desc">Цена (по убыванию)</option>
              <option value="area-asc">Площадь (по возрастанию)</option>
              <option value="area-desc">Площадь (по убыванию)</option>
              <option value="newest">Сначала новые</option>
            </select>
          </div>

          <button @click="resetFilters" class="catalog__reset-btn">Сбросить фильтры</button>
        </div>

        <div v-if="hasActiveFilters" class="catalog__active-filters">
          <span class="catalog__active-filters-label">Активные фильтры:</span>
          <div class="catalog__filter-tags">
            <span v-for="filter in activeFilters" :key="filter.key" class="catalog__filter-tag">
              {{ filter.label }}
              <button @click="removeFilter(filter.key)" class="catalog__filter-tag-remove">
                ×
              </button>
            </span>
          </div>
        </div>
      </div>

      <div v-if="filteredProperties.length" class="catalog__grid">
        <PropertyCard
          v-for="property in paginatedProperties"
          :key="property.id"
          :property="property"
          @toggle-favorite="toggleFavorite"
        />
      </div>

      <div v-else-if="loading" class="catalog__loading">
        <div class="catalog__spinner"></div>
        <p>Загрузка объектов...</p>
      </div>

      <div v-else class="catalog__empty">
        <span class="catalog__empty-icon">🏠</span>
        <h3 class="catalog__empty-title">Объекты не найдены</h3>
        <p class="catalog__empty-text">Попробуйте изменить параметры фильтрации</p>
        <button @click="resetFilters" class="catalog__empty-btn">Сбросить фильтры</button>
      </div>

      <div v-if="filteredProperties.length" class="catalog__pagination">
        <button
          class="catalog__pagination-btn"
          :disabled="currentPage === 1"
          @click="currentPage--"
        >
          ←
        </button>

        <span class="catalog__pagination-info">
          Страница {{ currentPage }} из {{ totalPages }}
        </span>

        <button
          class="catalog__pagination-btn"
          :disabled="currentPage === totalPages"
          @click="currentPage++"
        >
          →
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import PropertyCard from '@/components/PropertyCard.vue'

const loading = ref(true)
const properties = ref([])
const currentPage = ref(1)
const itemsPerPage = 9
const favorites = ref([])

const filters = ref({
  search: '',
  type: '',
  rooms: '',
  priceMin: null,
  priceMax: null,
  areaMin: null,
})

const sortBy = ref('newest')

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

const updateFavoritesStatus = () => {
  properties.value.forEach((property) => {
    property.isFavorite = favorites.value.some((fav) => fav.id === property.id)
  })
}

onMounted(() => {
  loadFavorites()

  setTimeout(() => {
    properties.value = mockProperties
    updateFavoritesStatus()
    loading.value = false
  }, 1000)
})

const toggleFavorite = (propertyId) => {
  const property = properties.value.find((p) => p.id === propertyId)
  if (property) {
    property.isFavorite = !property.isFavorite

    if (property.isFavorite) {
      if (!favorites.value.some((fav) => fav.id === propertyId)) {
        favorites.value.push({ ...property })
      }
    } else {
      favorites.value = favorites.value.filter((fav) => fav.id !== propertyId)
    }

    localStorage.setItem('favorites', JSON.stringify(favorites.value))

    console.log(property.isFavorite ? 'Добавлено в избранное' : 'Удалено из избранного')
  }
}

const mockProperties = [
  {
    id: 1,
    title: 'Уютная квартира в центре',
    address: 'ул. Ленина, 15',
    price: 7500000,
    area: 65,
    rooms: 2,
    type: 'apartment',
    image: '/image/home1.jpg',
    isFavorite: false,
    createdAt: '2024-01-15',
  },
  {
    id: 2,
    title: 'Просторный дом с участком',
    address: 'ул. Садовая, 25',
    price: 12500000,
    area: 150,
    rooms: 4,
    type: 'house',
    image: '/image/home2.jpg',
    isFavorite: false,
    createdAt: '2024-02-01',
  },
  {
    id: 3,
    title: 'Современная студия',
    address: 'ул. Новая, 5',
    price: 4500000,
    area: 35,
    rooms: 1,
    type: 'apartment',
    image: '/image/home3.jpg',
    isFavorite: false,
    createdAt: '2024-02-15',
  },
  {
    id: 4,
    title: 'Коммерческое помещение',
    address: 'пр. Мира, 100',
    price: 15000000,
    area: 200,
    rooms: 5,
    type: 'commercial',
    image: '/image/home4.jpg',
    isFavorite: false,
    createdAt: '2024-01-20',
  },
]

const filteredProperties = computed(() => {
  return properties.value
    .filter((property) => {
      if (filters.value.search) {
        const searchLower = filters.value.search.toLowerCase()
        const matchesSearch =
          property.title.toLowerCase().includes(searchLower) ||
          property.address.toLowerCase().includes(searchLower)
        if (!matchesSearch) return false
      }

      if (filters.value.type && property.type !== filters.value.type) return false

      if (filters.value.rooms) {
        if (filters.value.rooms === '4' && property.rooms < 4) return false
        else if (filters.value.rooms !== '4' && property.rooms !== parseInt(filters.value.rooms))
          return false
      }

      if (filters.value.priceMin && property.price < filters.value.priceMin) return false
      if (filters.value.priceMax && property.price > filters.value.priceMax) return false

      if (filters.value.areaMin && property.area < filters.value.areaMin) return false

      return true
    })
    .sort((a, b) => {
      switch (sortBy.value) {
        case 'price-asc':
          return a.price - b.price
        case 'price-desc':
          return b.price - a.price
        case 'area-asc':
          return a.area - b.area
        case 'area-desc':
          return b.area - a.area
        case 'newest':
          return new Date(b.createdAt) - new Date(a.createdAt)
        default:
          return 0
      }
    })
})

const totalPages = computed(() => Math.ceil(filteredProperties.value.length / itemsPerPage))

const paginatedProperties = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredProperties.value.slice(start, end)
})

const hasActiveFilters = computed(() => {
  return Object.values(filters.value).some((value) => value !== '' && value !== null)
})

const activeFilters = computed(() => {
  const active = []

  if (filters.value.search) {
    active.push({ key: 'search', label: `Поиск: ${filters.value.search}` })
  }
  if (filters.value.type) {
    const typeLabels = {
      apartment: 'Квартира',
      house: 'Дом',
      cottage: 'Коттедж',
      commercial: 'Коммерческая',
    }
    active.push({ key: 'type', label: `Тип: ${typeLabels[filters.value.type]}` })
  }
  if (filters.value.rooms) {
    const roomLabel = filters.value.rooms === '4' ? '4+ комнат' : `${filters.value.rooms} комн.`
    active.push({ key: 'rooms', label: roomLabel })
  }
  if (filters.value.priceMin) {
    active.push({ key: 'priceMin', label: `Цена от: ${filters.value.priceMin.toLocaleString()} ₽` })
  }
  if (filters.value.priceMax) {
    active.push({ key: 'priceMax', label: `Цена до: ${filters.value.priceMax.toLocaleString()} ₽` })
  }
  if (filters.value.areaMin) {
    active.push({ key: 'areaMin', label: `Площадь от: ${filters.value.areaMin} м²` })
  }

  return active
})

const resetFilters = () => {
  filters.value = {
    search: '',
    type: '',
    rooms: '',
    priceMin: null,
    priceMax: null,
    areaMin: null,
  }
  sortBy.value = 'newest'
  currentPage.value = 1
}

const removeFilter = (key) => {
  if (key.includes('price') || key.includes('area')) {
    filters.value[key] = null
  } else {
    filters.value[key] = ''
  }
  currentPage.value = 1
}

watch(
  [filters, sortBy],
  () => {
    currentPage.value = 1
  },
  { deep: true },
)
</script>

<style scoped>
.catalog {
  padding: var(--space-xxl) 0;
  min-height: 100vh;
  background-color: var(--color-background);
}

.catalog__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-l);
}

.catalog__header {
  text-align: center;
  margin-bottom: var(--space-xl);
}

.catalog__title {
  color: var(--color-text-dark);
  font-size: 2.5rem;
  margin-bottom: var(--space-s);
}

.catalog__subtitle {
  color: var(--color-text-light);
  font-size: 1.1rem;
}

.catalog__filters {
  background: var(--color-white);
  padding: var(--space-l);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  margin-bottom: var(--space-xl);
}

.catalog__filters-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: var(--space-m);
  margin-bottom: var(--space-m);
}

.catalog__filters-row:last-child {
  margin-bottom: 0;
}

.catalog__filter-group {
  display: flex;
  flex-direction: column;
}

.catalog__filter-group--search {
  grid-column: span 2;
}

.catalog__filter-label {
  font-size: 0.9rem;
  color: var(--color-text-light);
  margin-bottom: var(--space-xs);
}

.catalog__filter-input,
.catalog__filter-select {
  padding: var(--space-s) var(--space-m);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-small);
  font-size: 1rem;
  transition: all 0.3s;
  background: var(--color-white);
}

.catalog__filter-input:focus,
.catalog__filter-select:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(0, 176, 165, 0.1);
}

.catalog__reset-btn {
  align-self: flex-end;
  padding: var(--space-s) var(--space-m);
  background: none;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-small);
  color: var(--color-text-light);
  cursor: pointer;
  transition: all 0.3s;
  font-size: 0.9rem;
}

.catalog__reset-btn:hover {
  background: var(--color-background);
  border-color: var(--color-text-light);
}

.catalog__active-filters {
  margin-top: var(--space-m);
  padding-top: var(--space-m);
  border-top: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  gap: var(--space-m);
  flex-wrap: wrap;
}

.catalog__active-filters-label {
  color: var(--color-text-light);
  font-size: 0.9rem;
}

.catalog__filter-tags {
  display: flex;
  gap: var(--space-s);
  flex-wrap: wrap;
}

.catalog__filter-tag {
  display: inline-flex;
  align-items: center;
  gap: var(--space-xs);
  padding: var(--space-xs) var(--space-s);
  background: var(--color-primary);
  color: var(--color-white);
  border-radius: var(--radius-small);
  font-size: 0.9rem;
}

.catalog__filter-tag-remove {
  background: none;
  border: none;
  color: var(--color-white);
  cursor: pointer;
  font-size: 1.2rem;
  padding: 0 var(--space-xs);
  opacity: 0.8;
  transition: opacity 0.3s;
}

.catalog__filter-tag-remove:hover {
  opacity: 1;
}

.catalog__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: var(--space-l);
  margin-bottom: var(--space-xl);
}

.catalog__loading {
  text-align: center;
  padding: var(--space-xxl);
  color: var(--color-text-light);
}

.catalog__spinner {
  width: 50px;
  height: 50px;
  border: 3px solid var(--color-border);
  border-top-color: var(--color-primary);
  border-radius: 50%;
  margin: 0 auto var(--space-m);
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.catalog__empty {
  text-align: center;
  padding: var(--space-xxl);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
}

.catalog__empty-icon {
  font-size: 4rem;
  display: block;
  margin-bottom: var(--space-m);
  opacity: 0.5;
}

.catalog__empty-title {
  color: var(--color-text-dark);
  font-size: 1.5rem;
  margin-bottom: var(--space-s);
}

.catalog__empty-text {
  color: var(--color-text-light);
  margin-bottom: var(--space-l);
}

.catalog__empty-btn {
  padding: var(--space-s) var(--space-l);
  background: var(--color-primary);
  color: var(--color-white);
  border: none;
  border-radius: var(--radius-medium);
  cursor: pointer;
  transition: all 0.3s;
}

.catalog__empty-btn:hover {
  background: var(--color-primary-dark);
  transform: translateY(-2px);
}

.catalog__pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: var(--space-m);
}

.catalog__pagination-btn {
  padding: var(--space-s) var(--space-m);
  background: var(--color-white);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-small);
  color: var(--color-text-dark);
  cursor: pointer;
  transition: all 0.3s;
  font-size: 1rem;
}

.catalog__pagination-btn:hover:not(:disabled) {
  background: var(--color-primary);
  color: var(--color-white);
  border-color: var(--color-primary);
}

.catalog__pagination-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.catalog__pagination-info {
  color: var(--color-text-light);
}

@media (max-width: 768px) {
  .catalog__container {
    padding: 0 var(--space-m);
  }

  .catalog__title {
    font-size: 2rem;
  }

  .catalog__filters-row {
    grid-template-columns: 1fr;
  }

  .catalog__filter-group--search {
    grid-column: span 1;
  }

  .catalog__grid {
    grid-template-columns: 1fr;
  }
}
</style>
