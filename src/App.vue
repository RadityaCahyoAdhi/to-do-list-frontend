<template>
  <header>
    <div class="wrapper">
      <nav class="navbar">
        <div class="navbar-brand">
          <RouterLink to="/">To-Do List</RouterLink>
        </div>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<script>
import { defineComponent } from 'vue'
import { RouterLink, RouterView } from 'vue-router'

export default defineComponent({
  name: 'App',
  components: {
    RouterLink,
    RouterView,
  },
})
</script>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #fff;
  border-bottom: 2px solid var(--color-border);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  z-index: 10;
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.navbar-menu {
  display: flex;
  gap: 1rem;
}

.navbar-menu a {
  text-decoration: none;
  color: var(--color-text);
}

.navbar-menu a.router-link-exact-active {
  color: var(--color-primary);
}

.navbar-menu a.router-link-exact-active:hover {
  background-color: transparent;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
