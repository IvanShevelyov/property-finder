<template>
  <div class="app">
    <AppHeader />
    <main class="app__main">
      <router-view />
    </main>
    <AppFooter />
  </div>
</template>

<script>
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
  },
}
</script>

<style>
/* CSS переменные (как в БЭМ) */
:root {
  /* Цвета */
  --color-primary: #00b0a5;
  --color-primary-dark: #008c83;
  --color-secondary: #2c3e50;
  --color-accent: #ff6b6b;
  --color-dark: #2c3e50;
  --color-white: #ffffff;
  --color-background: #f8f9fa;
  --color-border: #e0e0e0;
  --color-border-dark: #34495e;

  /* Текст */
  --color-text-dark: #2c3e50;
  --color-text-light: #7f8c8d;
  --color-text-muted: #bdc3c7;

  /* Тени */
  --shadow-light: 0 2px 10px rgba(0, 0, 0, 0.1);
  --shadow-medium: 0 4px 20px rgba(0, 0, 0, 0.1);
  --shadow-heavy: 0 8px 25px rgba(0, 0, 0, 0.1);

  /* Радиусы */
  --radius-small: 6px;
  --radius-medium: 8px;
  --radius-large: 12px;

  /* Отступы */
  --space-xs: 4px;
  --space-s: 8px;
  --space-m: 16px;
  --space-l: 24px;
  --space-xl: 32px;
  --space-xxl: 40px;
}

/* Сброс стилей */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family:
    'Inter',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    sans-serif;
  line-height: 1.6;
  color: var(--color-text-dark);
  background-color: var(--color-background);
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.app__main {
  flex: 1;
}

/* Глобальные утилитные классы */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Общие стили для кнопок */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  border: none;
  border-radius: var(--radius-small);
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
  text-decoration: none;
  text-align: center;
  font-family: inherit;
}

.btn--primary {
  background: var(--color-primary);
  color: var(--color-white);
}

.btn--primary:hover {
  background: var(--color-primary-dark);
  transform: translateY(-1px);
}

.btn--secondary {
  background: var(--color-secondary);
  color: var(--color-white);
}

.btn--secondary:hover {
  background: var(--color-dark);
}

.btn--outline {
  background: transparent;
  color: var(--color-primary);
  border: 2px solid var(--color-primary);
}

.btn--outline:hover {
  background: var(--color-primary);
  color: var(--color-white);
}

.btn--large {
  padding: 16px 32px;
  font-size: 1.1rem;
}

.btn--small {
  padding: 8px 16px;
  font-size: 0.9rem;
}

/* Адаптивные изображения */
.img-responsive {
  max-width: 100%;
  height: auto;
  display: block;
}
</style>
