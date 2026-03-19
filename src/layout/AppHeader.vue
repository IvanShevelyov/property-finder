<template>
  <header class="header">
    <div class="header__container container">
      <div class="header__content">
        <div class="header__logo">
          <router-link to="/" class="header__logo-link">
            <span class="header__logo-icon">🏠</span>
            <span class="header__logo-text">PropertyFinder</span>
          </router-link>
        </div>

        <nav class="header__nav">
          <router-link to="/catalog" class="header__nav-link">Каталог</router-link>
          <router-link to="/favorites" class="header__nav-link">Избранное</router-link>
          <router-link to="/about" class="header__nav-link">О компании</router-link>
        </nav>

        <div class="header__actions">
          <button class="header__phone-btn">📞 +7 (999) 123-45-67</button>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup></script>

<style scoped>
.header {
  background: var(--color-white);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 1000;
}

.header__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.header__content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 0;
}

.header__logo-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: var(--color-text-dark);
  font-weight: bold;
  font-size: 1.5rem;
}

.header__logo-icon {
  font-size: 2rem;
  margin-right: 8px;
}

.header__logo-text {
  color: var(--color-primary);
}

.header__nav {
  display: flex;
  gap: 32px;
}

.header__nav-link {
  text-decoration: none;
  color: var(--color-text-dark);
  font-weight: 500;
  padding: 8px 0;
  position: relative;
  transition: color 0.2s;
}

.header__nav-link:hover {
  color: var(--color-primary);
}

.header__nav-link--active {
  color: var(--color-primary);
}

.header__nav-link--active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--color-primary);
}

.header__phone-btn {
  background: var(--color-primary);
  color: var(--color-white);
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s;
}

.header__phone-btn:hover {
  background: var(--color-primary-dark);
}

.header--transparent {
  background: transparent;
  box-shadow: none;
}

@media (max-width: 768px) {
  .header__content {
    flex-direction: column;
    gap: 16px;
    text-align: center;
  }

  .header__nav {
    gap: 20px;
  }

  .header__logo-text {
    font-size: 1.3rem;
  }
}
</style>
