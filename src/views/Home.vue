<template>
  <div class="home">
    <section class="hero">
      <div class="hero__container">
        <div class="hero__content">
          <h1 class="hero__title">Найдите свой идеальный дом</h1>
          <p class="hero__subtitle">Более 1000 объектов недвижимости по всей России</p>

          <form class="hero__search-form" @submit.prevent="handleSearch">
            <input
              type="text"
              v-model="searchQuery"
              placeholder="Введите район, улицу или станцию метро..."
              class="hero__search-input"
            />
            <button type="submit" class="hero__search-btn">
              <span class="hero__search-btn-text">Найти</span>
              <span class="hero__search-btn-icon">→</span>
            </button>
          </form>

          <div class="hero__quick-links">
            <span class="hero__quick-links-label">Популярные запросы:</span>
            <button
              v-for="query in popularQueries"
              :key="query"
              @click="searchQuery = query"
              class="hero__quick-link"
            >
              {{ query }}
            </button>
          </div>
        </div>
      </div>

      <div class="hero__decoration">
        <div class="hero__decoration-circle"></div>
        <div class="hero__decoration-circle hero__decoration-circle--small"></div>
      </div>
    </section>

    <section class="categories">
      <div class="categories__container">
        <div class="categories__header">
          <h2 class="categories__title">Популярные категории</h2>
          <p class="categories__subtitle">Выберите тип недвижимости, который вас интересует</p>
        </div>

        <div class="categories__grid">
          <div
            v-for="category in categories"
            :key="category.id"
            class="category-card"
            @click="navigateToCatalog(category.filter)"
          >
            <div class="category-card__icon">{{ category.icon }}</div>
            <h3 class="category-card__title">{{ category.title }}</h3>
            <p class="category-card__price">{{ category.price }}</p>
            <span class="category-card__count">{{ category.count }} объектов</span>
          </div>
        </div>
      </div>
    </section>

    <section class="features">
      <div class="features__container">
        <h2 class="features__title">Почему выбирают нас</h2>
        <div class="features__grid">
          <div v-for="feature in features" :key="feature.id" class="feature-card">
            <div class="feature-card__icon-wrapper">
              <span class="feature-card__icon">{{ feature.icon }}</span>
            </div>
            <h3 class="feature-card__title">{{ feature.title }}</h3>
            <p class="feature-card__description">{{ feature.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <section class="featured">
      <div class="featured__container">
        <div class="featured__header">
          <h2 class="featured__title">Рекомендуемые объекты</h2>
          <p class="featured__subtitle">Специально подобранные для вас варианты</p>
        </div>

        <div v-if="loading" class="featured__loading">
          <div class="featured__spinner"></div>
        </div>

        <div v-else class="featured__grid">
          <PropertyCard
            v-for="property in featuredProperties"
            :key="property.id"
            :property="property"
            @toggle-favorite="toggleFavorite"
          />
        </div>

        <div class="featured__actions">
          <router-link to="/catalog" class="featured__btn"> Смотреть все объекты </router-link>
        </div>
      </div>
    </section>

    <section class="stats">
      <div class="stats__container">
        <div class="stats__grid">
          <div v-for="stat in stats" :key="stat.id" class="stat-item">
            <span class="stat-item__number">{{ stat.number }}</span>
            <span class="stat-item__label">{{ stat.label }}</span>
          </div>
        </div>
      </div>
    </section>

    <section class="how-it-works">
      <div class="how-it-works__container">
        <h2 class="how-it-works__title">Как это работает</h2>
        <div class="how-it-works__grid">
          <div v-for="step in steps" :key="step.id" class="step-card">
            <div class="step-card__number">{{ step.id }}</div>
            <div class="step-card__icon">{{ step.icon }}</div>
            <h3 class="step-card__title">{{ step.title }}</h3>
            <p class="step-card__description">{{ step.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <section class="cta">
      <div class="cta__container">
        <h2 class="cta__title">Готовы найти свой идеальный дом?</h2>
        <p class="cta__subtitle">Начните поиск прямо сейчас</p>
        <router-link to="/catalog" class="cta__btn"> Перейти в каталог </router-link>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import PropertyCard from '@/components/PropertyCard.vue'

const router = useRouter()

const searchQuery = ref('')
const loading = ref(true)
const featuredProperties = ref([])

const popularQueries = ['Центр', 'Новостройки', 'У моря', 'С ремонтом']

const categories = [
  {
    id: 1,
    icon: '🏢',
    title: 'Квартиры',
    price: 'от 2 млн ₽',
    count: 456,
    filter: { type: 'apartment' },
  },
  {
    id: 2,
    icon: '🏠',
    title: 'Дома',
    price: 'от 5 млн ₽',
    count: 234,
    filter: { type: 'house' },
  },
  {
    id: 3,
    icon: '🏡',
    title: 'Коттеджи',
    price: 'от 8 млн ₽',
    count: 89,
    filter: { type: 'cottage' },
  },
  {
    id: 4,
    icon: '🏪',
    title: 'Коммерческая',
    price: 'от 3 млн ₽',
    count: 123,
    filter: { type: 'commercial' },
  },
]

// Преимущества
const features = [
  {
    id: 1,
    icon: '✅',
    title: 'Проверенные объекты',
    description: 'Все объекты проходят тщательную проверку перед публикацией',
  },
  {
    id: 2,
    icon: '⚡',
    title: 'Быстрый поиск',
    description: 'Умные фильтры помогут найти подходящий вариант за минуту',
  },
  {
    id: 3,
    icon: '🤝',
    title: 'Честные цены',
    description: 'Работаем напрямую с собственниками, без посредников',
  },
  {
    id: 4,
    icon: '🔒',
    title: 'Безопасная сделка',
    description: 'Юридическое сопровождение на всех этапах',
  },
]

const stats = [
  {
    id: 1,
    number: '1000+',
    label: 'Объектов',
  },
  {
    id: 2,
    number: '5000+',
    label: 'Довольных клиентов',
  },
  {
    id: 3,
    number: '5 лет',
    label: 'На рынке',
  },
  {
    id: 4,
    number: '24/7',
    label: 'Поддержка',
  },
]

const steps = [
  {
    id: 1,
    icon: '🔍',
    title: 'Поиск',
    description: 'Найдите подходящий объект с помощью удобных фильтров',
  },
  {
    id: 2,
    icon: '❤️',
    title: 'Избранное',
    description: 'Сохраняйте понравившиеся варианты в избранное',
  },
  {
    id: 3,
    icon: '📞',
    title: 'Контакт',
    description: 'Свяжитесь с собственником или агентом',
  },
  {
    id: 4,
    icon: '🏠',
    title: 'Просмотр',
    description: 'Осмотрите объект лично и примите решение',
  },
]

onMounted(() => {
  setTimeout(() => {
    featuredProperties.value = [
      {
        id: 1,
        title: 'Уютная квартира в центре',
        address: 'ул. Ленина, 15',
        price: 7500000,
        area: 65,
        rooms: 2,
        type: 'apartment',
        image: '../public/image/home1.jpg',
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
        image: '../public/image/home2.jpg',
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
        image: '../public/image/home3.jpg',
        isFavorite: false,
        createdAt: '2024-02-15',
      },
    ]
    loading.value = false
  }, 1000)
})

const handleSearch = () => {
  if (searchQuery.value.trim()) {
    router.push({
      path: '/catalog',
      query: { search: searchQuery.value },
    })
  }
}

const navigateToCatalog = (filter) => {
  router.push({
    path: '/catalog',
    query: filter,
  })
}

const toggleFavorite = (propertyId) => {
  const property = featuredProperties.value.find((p) => p.id === propertyId)
  if (property) {
    property.isFavorite = !property.isFavorite

    console.log('Toggle favorite:', propertyId)
  }
}
</script>

<style scoped>
.hero {
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-dark) 100%);
  color: var(--color-white);
  padding: 80px 0;
  position: relative;
  overflow: hidden;
}

.hero__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  position: relative;
  z-index: 2;
}

.hero__content {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.hero__title {
  font-size: 3rem;
  margin-bottom: var(--space-m);
  font-weight: 700;
  line-height: 1.2;
  animation: fadeInUp 0.5s ease;
}

.hero__subtitle {
  font-size: 1.2rem;
  margin-bottom: var(--space-xxl);
  opacity: 0.9;
  line-height: 1.5;
  animation: fadeInUp 0.5s ease 0.2s both;
}

.hero__search-form {
  display: flex;
  max-width: 600px;
  margin: 0 auto var(--space-l);
  background: var(--color-white);
  border-radius: var(--radius-medium);
  overflow: hidden;
  box-shadow: var(--shadow-heavy);
  animation: fadeInUp 0.5s ease 0.4s both;
}

.hero__search-input {
  flex: 1;
  padding: var(--space-l) var(--space-xl);
  border: none;
  font-size: 1rem;
  outline: none;
  color: var(--color-text-dark);
}

.hero__search-input::placeholder {
  color: var(--color-text-light);
}

.hero__search-btn {
  padding: var(--space-l) var(--space-xl);
  background: var(--color-secondary);
  color: var(--color-white);
  border: none;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: var(--space-xs);
}

.hero__search-btn:hover {
  background: var(--color-dark);
}

.hero__search-btn-icon {
  font-size: 1.2rem;
  transition: transform 0.3s;
}

.hero__search-btn:hover .hero__search-btn-icon {
  transform: translateX(5px);
}

.hero__quick-links {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-m);
  flex-wrap: wrap;
  animation: fadeInUp 0.5s ease 0.6s both;
}

.hero__quick-links-label {
  font-size: 0.9rem;
  opacity: 0.8;
}

.hero__quick-link {
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: var(--color-white);
  padding: var(--space-xs) var(--space-m);
  border-radius: 30px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.3s;
}

.hero__quick-link:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
}

.hero__decoration {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
}

.hero__decoration-circle {
  position: absolute;
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  top: -150px;
  right: -150px;
}

.hero__decoration-circle--small {
  width: 200px;
  height: 200px;
  bottom: -100px;
  left: -100px;
  top: auto;
  right: auto;
}

/* Категории */
.categories {
  padding: var(--space-xxl) 0;
  background: var(--color-white);
}

.categories__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.categories__header {
  text-align: center;
  margin-bottom: var(--space-xxl);
}

.categories__title {
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-s);
}

.categories__subtitle {
  color: var(--color-text-light);
  font-size: 1.1rem;
}

.categories__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-xl);
}

.category-card {
  text-align: center;
  padding: var(--space-xxl) var(--space-l);
  background: var(--color-white);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-large);
  transition: all 0.3s;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.category-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, var(--color-primary), var(--color-secondary));
  transform: translateX(-100%);
  transition: transform 0.3s;
}

.category-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-heavy);
  border-color: var(--color-primary);
}

.category-card:hover::before {
  transform: translateX(0);
}

.category-card__icon {
  font-size: 3rem;
  margin-bottom: var(--space-l);
}

.category-card__title {
  color: var(--color-text-dark);
  margin-bottom: var(--space-xs);
  font-size: 1.3rem;
}

.category-card__price {
  color: var(--color-primary);
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: var(--space-s);
}

.category-card__count {
  color: var(--color-text-light);
  font-size: 0.9rem;
}

/* Преимущества */
.features {
  padding: var(--space-xxl) 0;
  background: var(--color-background);
}

.features__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.features__title {
  text-align: center;
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-xxl);
}

.features__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-xl);
}

.feature-card {
  text-align: center;
  padding: var(--space-xl);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  transition: transform 0.3s;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-card__icon-wrapper {
  width: 80px;
  height: 80px;
  margin: 0 auto var(--space-l);
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-dark) 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.feature-card__icon {
  font-size: 2rem;
  color: var(--color-white);
}

.feature-card__title {
  color: var(--color-text-dark);
  font-size: 1.2rem;
  margin-bottom: var(--space-s);
}

.feature-card__description {
  color: var(--color-text-light);
  line-height: 1.5;
}

/* Рекомендуемые объекты */
.featured {
  padding: var(--space-xxl) 0;
  background: var(--color-white);
}

.featured__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.featured__header {
  text-align: center;
  margin-bottom: var(--space-xxl);
}

.featured__title {
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-s);
}

.featured__subtitle {
  color: var(--color-text-light);
  font-size: 1.1rem;
}

.featured__loading {
  text-align: center;
  padding: var(--space-xxl);
}

.featured__spinner {
  width: 50px;
  height: 50px;
  border: 3px solid var(--color-border);
  border-top-color: var(--color-primary);
  border-radius: 50%;
  margin: 0 auto;
  animation: spin 1s linear infinite;
}

.featured__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: var(--space-l);
  margin-bottom: var(--space-xl);
}

.featured__actions {
  text-align: center;
}

.featured__btn {
  display: inline-block;
  padding: var(--space-m) var(--space-xl);
  background: transparent;
  color: var(--color-primary);
  border: 2px solid var(--color-primary);
  border-radius: var(--radius-medium);
  font-size: 1.1rem;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s;
}

.featured__btn:hover {
  background: var(--color-primary);
  color: var(--color-white);
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

/* Статистика */
.stats {
  padding: var(--space-xxl) 0;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-dark) 100%);
  color: var(--color-white);
}

.stats__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.stats__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: var(--space-xl);
  text-align: center;
}

.stat-item__number {
  display: block;
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: var(--space-xs);
}

.stat-item__label {
  font-size: 1.1rem;
  opacity: 0.9;
}

/* Как это работает */
.how-it-works {
  padding: var(--space-xxl) 0;
  background: var(--color-background);
}

.how-it-works__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.how-it-works__title {
  text-align: center;
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-xxl);
}

.how-it-works__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-xl);
}

.step-card {
  text-align: center;
  padding: var(--space-xl);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  position: relative;
}

.step-card__number {
  position: absolute;
  top: -15px;
  left: 50%;
  transform: translateX(-50%);
  width: 30px;
  height: 30px;
  background: var(--color-primary);
  color: var(--color-white);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
}

.step-card__icon {
  font-size: 2.5rem;
  margin-bottom: var(--space-m);
  margin-top: var(--space-s);
}

.step-card__title {
  color: var(--color-text-dark);
  font-size: 1.2rem;
  margin-bottom: var(--space-s);
}

.step-card__description {
  color: var(--color-text-light);
  line-height: 1.5;
}

.cta {
  padding: var(--space-xxl) 0;
  background: linear-gradient(135deg, var(--color-secondary) 0%, var(--color-dark) 100%);
  color: var(--color-white);
  text-align: center;
}

.cta__container {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 20px;
}

.cta__title {
  font-size: 2.5rem;
  margin-bottom: var(--space-m);
}

.cta__subtitle {
  font-size: 1.2rem;
  margin-bottom: var(--space-xl);
  opacity: 0.9;
}

.cta__btn {
  display: inline-block;
  padding: var(--space-m) var(--space-xl);
  background: var(--color-white);
  color: var(--color-primary);
  border-radius: var(--radius-medium);
  font-size: 1.1rem;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s;
}

.cta__btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-heavy);
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

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

@media (max-width: 768px) {
  .hero__title {
    font-size: 2rem;
  }

  .hero__subtitle {
    font-size: 1rem;
    margin-bottom: var(--space-xl);
  }

  .hero__search-form {
    flex-direction: column;
  }

  .hero__search-btn {
    justify-content: center;
  }

  .hero__quick-links {
    flex-direction: column;
    gap: var(--space-xs);
  }

  .categories__title,
  .features__title,
  .featured__title,
  .how-it-works__title,
  .cta__title {
    font-size: 2rem;
  }

  .stats__grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .stat-item__number {
    font-size: 2rem;
  }
}

@media (max-width: 480px) {
  .hero {
    padding: 60px 0;
  }

  .categories__grid,
  .features__grid,
  .featured__grid,
  .how-it-works__grid {
    grid-template-columns: 1fr;
  }

  .stats__grid {
    grid-template-columns: 1fr;
    gap: var(--space-l);
  }
}
</style>
