<template>
  <footer class="footer">
    <div class="footer__container container">
      <div class="footer__content">
        <div class="footer__section">
          <div class="footer__logo">
            <span class="footer__logo-icon">🏠</span>
            <span class="footer__logo-text">PropertyFinder</span>
          </div>
          <p class="footer__description">
            Современная платформа для поиска и аренды недвижимости. Мы помогаем найти идеальный дом
            каждому.
          </p>
        </div>

        <div class="footer__section">
          <h4 class="footer__title">Навигация</h4>
          <ul class="footer__links">
            <li class="footer__links-item">
              <router-link to="/catalog" class="footer__link">Каталог</router-link>
            </li>
            <li class="footer__links-item">
              <router-link to="/favorites" class="footer__link">Избранное</router-link>
            </li>
            <li class="footer__links-item">
              <router-link to="/about" class="footer__link">О компании</router-link>
            </li>
          </ul>
        </div>

        <div class="footer__section">
          <h4 class="footer__title">Контакты</h4>
          <div class="footer__contacts">
            <p class="footer__contact-item">📞 +7 (999) 123-45-67</p>
            <p class="footer__contact-item">📧 info@propertyfinder.ru</p>
            <p class="footer__contact-item">📍 г. Москва, ул. Примерная, 123</p>
          </div>
        </div>
      </div>

      <div class="footer__bottom">
        <p class="footer__copyright">&copy; 2024 PropertyFinder. Все права защищены.</p>
      </div>
    </div>
  </footer>
</template>

<script setup></script>

<style scoped>
.footer {
  background: var(--color-dark);
  color: var(--color-white);
  margin-top: auto;
}

.footer__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.footer__content {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 40px;
  padding: 40px 0;
}

.footer__section {
  display: flex;
  flex-direction: column;
}

.footer__title {
  color: var(--color-primary);
  margin-bottom: 16px;
  font-size: 1.1rem;
}

.footer__description {
  line-height: 1.6;
  color: var(--color-text-light);
  margin-top: 12px;
}

.footer__links {
  list-style: none;
  margin: 0;
  padding: 0;
}

.footer__links-item {
  margin-bottom: 8px;
}

.footer__link {
  color: var(--color-text-light);
  text-decoration: none;
  transition: color 0.2s;
}

.footer__link:hover {
  color: var(--color-primary);
}

.footer__contacts {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.footer__contact-item {
  color: var(--color-text-light);
  margin: 0;
}

.footer__bottom {
  border-top: 1px solid var(--color-border-dark);
  padding: 20px 0;
  text-align: center;
}

.footer__copyright {
  color: var(--color-text-light);
  margin: 0;
}

.footer__logo {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.footer__logo-icon {
  font-size: 1.5rem;
  margin-right: 8px;
}

.footer__logo-text {
  color: var(--color-primary);
  font-weight: bold;
  font-size: 1.3rem;
}

.footer--compact .footer__content {
  padding: 20px 0;
}

@media (max-width: 768px) {
  .footer__content {
    grid-template-columns: 1fr;
    gap: 30px;
    text-align: center;
  }

  .footer__logo {
    justify-content: center;
  }
}
</style>
