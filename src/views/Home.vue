<template>
  <div class="home">
    <!-- Герой-секция -->
    <section class="hero">
      <div class="hero__container container">
        <div class="hero__content">
          <h1 class="hero__title">Найдите свой идеальный дом</h1>
          <p class="hero__subtitle">Более 1000 объектов недвижимости по всей России</p>

          <!-- Поисковая форма -->
          <form class="hero__search-form">
            <input
              type="text"
              placeholder="Введите район, улицу или станцию метро..."
              class="hero__search-input"
            />
            <button type="submit" class="hero__search-btn btn btn--primary">Найти</button>
          </form>
        </div>
      </div>
    </section>

    <!-- Популярные категории -->
    <section class="categories">
      <div class="categories__container container">
        <h2 class="categories__title section-title">Популярные категории</h2>
        <div class="categories__grid">
          <div class="category-card">
            <div class="category-card__icon">🏢</div>
            <h3 class="category-card__title">Квартиры</h3>
            <p class="category-card__price">от 2 млн ₽</p>
          </div>
          <div class="category-card">
            <div class="category-card__icon">🏠</div>
            <h3 class="category-card__title">Дома</h3>
            <p class="category-card__price">от 5 млн ₽</p>
          </div>
          <div class="category-card">
            <div class="category-card__icon">🏪</div>
            <h3 class="category-card__title">Коммерческая</h3>
            <p class="category-card__price">от 3 млн ₽</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
// Логика главной страницы будет добавляться
</script>

<style scoped>
.hero {
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-dark) 100%);
  color: var(--color-white);
  padding: 80px 0;
  text-align: center;
}

.hero__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.hero__content {
  max-width: 800px;
  margin: 0 auto;
}

.hero__title {
  font-size: 3rem;
  margin-bottom: var(--space-m);
  font-weight: 700;
  line-height: 1.2;
}

.hero__subtitle {
  font-size: 1.2rem;
  margin-bottom: var(--space-xxl);
  opacity: 0.9;
  line-height: 1.5;
}

.hero__search-form {
  display: flex;
  max-width: 600px;
  margin: 0 auto;
  background: var(--color-white);
  border-radius: var(--radius-medium);
  overflow: hidden;
  box-shadow: var(--shadow-medium);
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
  border-radius: 0;
  white-space: nowrap;
}

.categories {
  padding: var(--space-xxl) 0;
  background: var(--color-white);
}

.categories__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.categories__title {
  text-align: center;
  font-size: 2.5rem;
  color: var(--color-text-dark);
  margin-bottom: var(--space-xxl);
}

.categories__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-xl);
  max-width: 900px;
  margin: 0 auto;
}

.category-card {
  text-align: center;
  padding: var(--space-xxl) var(--space-l);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-large);
  transition:
    transform 0.2s,
    box-shadow 0.2s;
  cursor: pointer;
}

.category-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-heavy);
}

.category-card__icon {
  font-size: 3rem;
  margin-bottom: var(--space-l);
}

.category-card__title {
  color: var(--color-text-dark);
  margin-bottom: var(--space-s);
  font-size: 1.3rem;
}

.category-card__price {
  color: var(--color-text-light);
  font-size: 1.1rem;
}

/* Модификаторы для hero */
.hero--small {
  padding: 40px 0;
}

.hero--small .hero__title {
  font-size: 2rem;
}

/* Утилитный класс для заголовков секций */
.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--color-text-dark);
  text-align: center;
  margin-bottom: var(--space-xxl);
}

/* Адаптивность */
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
    border-radius: var(--radius-medium);
  }

  .hero__search-input {
    border-radius: var(--radius-medium) var(--radius-medium) 0 0;
    padding: var(--space-l);
  }

  .hero__search-btn {
    border-radius: 0 0 var(--radius-medium) var(--radius-medium);
  }

  .categories__title {
    font-size: 2rem;
  }

  .section-title {
    font-size: 2rem;
  }

  .category-card {
    padding: var(--space-xl) var(--space-m);
  }
}

@media (max-width: 480px) {
  .hero {
    padding: 60px 0;
  }

  .categories {
    padding: var(--space-xl) 0;
  }

  .categories__grid {
    grid-template-columns: 1fr;
  }
}
</style>
