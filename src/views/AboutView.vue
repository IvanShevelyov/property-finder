<template>
  <div class="about">
    <div class="about__container">
      <section class="about__hero">
        <h1 class="about__title">О компании</h1>
        <p class="about__subtitle">
          PropertyFinder - современная платформа для поиска недвижимости
        </p>
        <p class="about__description">Мы помогаем людям находить идеальные дома с 2020 года</p>
      </section>

      <section class="about__mission">
        <div class="about__mission-content">
          <h2 class="about__section-title">Наша миссия</h2>
          <p class="about__text">
            Сделать поиск недвижимости простым, прозрачным и приятным. Мы соединяем людей с их
            будущим домом, используя современные технологии и индивидуальный подход.
          </p>
        </div>

        <div class="about__stats">
          <div class="about__stat-item">
            <span class="about__stat-number">500+</span>
            <span class="about__stat-label">Объектов</span>
          </div>
          <div class="about__stat-item">
            <span class="about__stat-number">1000+</span>
            <span class="about__stat-label">Клиентов</span>
          </div>
          <div class="about__stat-item">
            <span class="about__stat-number">4+</span>
            <span class="about__stat-label">Года</span>
          </div>
        </div>
      </section>

      <section class="about__advantages">
        <h2 class="about__section-title">Почему выбирают нас</h2>

        <div class="about__advantages-grid">
          <div v-for="advantage in advantages" :key="advantage.id" class="about__advantage-card">
            <div class="about__advantage-icon">{{ advantage.icon }}</div>
            <h3 class="about__advantage-title">{{ advantage.title }}</h3>
            <p class="about__advantage-description">{{ advantage.description }}</p>
          </div>
        </div>
      </section>

      <section class="about__team">
        <h2 class="about__section-title">Наша команда</h2>

        <div class="about__team-grid">
          <div v-for="member in team" :key="member.id" class="about__team-card">
            <div class="about__team-photo">
              <div class="about__team-initials">{{ member.initials }}</div>
            </div>
            <h3 class="about__team-name">{{ member.name }}</h3>
            <p class="about__team-position">{{ member.position }}</p>
          </div>
        </div>
      </section>

      <section class="about__contacts">
        <h2 class="about__section-title">Свяжитесь с нами</h2>

        <div class="about__contacts-grid">
          <div class="about__contact-item">
            <span class="about__contact-icon">📞</span>
            <div>
              <h4>Телефон</h4>
              <a href="tel:+74951234567" class="about__contact-link">+7 (495) 123-45-67</a>
            </div>
          </div>

          <div class="about__contact-item">
            <span class="about__contact-icon">✉️</span>
            <div>
              <h4>Email</h4>
              <a href="mailto:info@propertyfinder.ru" class="about__contact-link"
                >info@propertyfinder.ru</a
              >
            </div>
          </div>

          <div class="about__contact-item">
            <span class="about__contact-icon">📍</span>
            <div>
              <h4>Адрес</h4>
              <p>г. Москва, ул. Примерная, д. 123</p>
            </div>
          </div>
        </div>
      </section>

      <section class="about__cta">
        <h2 class="about__cta-title">Готовы найти свой идеальный дом?</h2>
        <router-link to="/catalog" class="about__cta-button"> Перейти в каталог </router-link>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const advantages = ref([
  {
    id: 1,
    icon: '🏠',
    title: 'Большой выбор',
    description: 'Более 500 проверенных объектов в разных районах города',
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
    title: 'Безопасность',
    description: 'Проверяем все документы и историю объектов',
  },
])

const team = ref([
  {
    id: 1,
    name: 'Анна Петрова',
    position: 'Основатель',
    initials: 'АП',
  },
  {
    id: 2,
    name: 'Иван Сидоров',
    position: 'Ведущий риелтор',
    initials: 'ИС',
  },
  {
    id: 3,
    name: 'Елена Иванова',
    position: 'Менеджер по работе с клиентами',
    initials: 'ЕИ',
  },
])
</script>

<style scoped>
.about {
  padding: var(--space-xxl) 0;
  background-color: var(--color-background);
}

.about__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-l);
}

.about__hero {
  text-align: center;
  margin-bottom: var(--space-xxl);
  padding: var(--space-xl);
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  border-radius: var(--radius-large);
  color: var(--color-white);
}

.about__title {
  color: var(--color-white);
  margin-bottom: var(--space-m);
  font-size: 2.5rem;
}

.about__subtitle {
  font-size: 1.5rem;
  margin-bottom: var(--space-m);
  opacity: 0.95;
}

.about__description {
  color: var(--color-white);
  font-size: 1.2rem;
  opacity: 0.9;
  max-width: 600px;
  margin: 0 auto;
}

.about__section-title {
  color: var(--color-text-dark);
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: var(--space-xl);
}

.about__mission {
  margin-bottom: var(--space-xxl);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-xl);
  align-items: center;
}

.about__mission-content {
  padding: var(--space-l);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
}

.about__text {
  color: var(--color-text-light);
  font-size: 1.1rem;
  line-height: 1.6;
  margin-top: var(--space-m);
}

.about__stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-l);
  text-align: center;
}

.about__stat-item {
  padding: var(--space-l);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

.about__stat-item:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

.about__stat-number {
  display: block;
  font-size: 2.5rem;
  font-weight: bold;
  color: var(--color-primary);
  margin-bottom: var(--space-xs);
}

.about__stat-label {
  color: var(--color-text-light);
  font-size: 1rem;
}

.about__advantages {
  margin-bottom: var(--space-xxl);
}

.about__advantages-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-l);
}

.about__advantage-card {
  padding: var(--space-l);
  background: var(--color-white);
  border-radius: var(--radius-large);
  text-align: center;
  box-shadow: var(--shadow-light);
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

.about__advantage-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-medium);
}

.about__advantage-icon {
  font-size: 3rem;
  margin-bottom: var(--space-m);
}

.about__advantage-title {
  color: var(--color-text-dark);
  font-size: 1.3rem;
  margin-bottom: var(--space-s);
}

.about__advantage-description {
  color: var(--color-text-light);
  line-height: 1.5;
}

.about__team {
  margin-bottom: var(--space-xxl);
}

.about__team-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-l);
}

.about__team-card {
  text-align: center;
  background: var(--color-white);
  padding: var(--space-l);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

.about__team-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-medium);
}

.about__team-photo {
  width: 150px;
  height: 150px;
  margin: 0 auto var(--space-m);
  border-radius: 50%;
  overflow: hidden;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  box-shadow: var(--shadow-light);
}

.about__team-initials {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-white);
  font-size: 2.5rem;
  font-weight: bold;
}

.about__team-name {
  color: var(--color-text-dark);
  font-size: 1.3rem;
  margin-bottom: var(--space-xs);
}

.about__team-position {
  color: var(--color-text-light);
  font-size: 1rem;
}

.about__contacts {
  margin-bottom: var(--space-xxl);
}

.about__contacts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-l);
}

.about__contact-item {
  display: flex;
  align-items: center;
  gap: var(--space-m);
  padding: var(--space-l);
  background: var(--color-white);
  border-radius: var(--radius-large);
  box-shadow: var(--shadow-light);
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

.about__contact-item:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

.about__contact-icon {
  font-size: 2rem;
  min-width: 48px;
  text-align: center;
}

.about__contact-item h4 {
  color: var(--color-text-dark);
  font-size: 1.1rem;
  margin-bottom: var(--space-xs);
}

.about__contact-link {
  color: var(--color-primary);
  text-decoration: none;
  transition: color 0.3s;
}

.about__contact-link:hover {
  color: var(--color-primary-dark);
}

.about__cta {
  text-align: center;
  padding: var(--space-xl);
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  border-radius: var(--radius-large);
  color: var(--color-white);
  box-shadow: var(--shadow-medium);
}

.about__cta-title {
  color: var(--color-white);
  font-size: 2rem;
  margin-bottom: var(--space-l);
}

.about__cta-button {
  display: inline-block;
  padding: var(--space-m) var(--space-xl);
  background: var(--color-white);
  color: var(--color-primary);
  text-decoration: none;
  border-radius: var(--radius-medium);
  font-size: 1.2rem;
  font-weight: 600;
  transition:
    transform 0.3s,
    box-shadow 0.3s,
    background-color 0.3s;
}

.about__cta-button:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-heavy);
  background-color: var(--color-background);
}

@media (max-width: 768px) {
  .about__container {
    padding: 0 var(--space-m);
  }

  .about__title {
    font-size: 2rem;
  }

  .about__subtitle {
    font-size: 1.2rem;
  }

  .about__mission {
    grid-template-columns: 1fr;
    gap: var(--space-l);
  }

  .about__section-title {
    font-size: 2rem;
  }

  .about__stats {
    grid-template-columns: 1fr;
    gap: var(--space-m);
  }

  .about__stat-number {
    font-size: 2rem;
  }

  .about__cta-title {
    font-size: 1.5rem;
  }

  .about__cta {
    padding: var(--space-l);
  }
}
</style>
