<script setup>
// Import des composants nécessaires de vue-router et des composants locaux
import { RouterLink, RouterView } from 'vue-router';
import HelloWorld from './components/HelloWorld.vue';
import CategorieView from "@/views/CategorieView.vue";
import ProductView from "@/views/ProductView.vue";
</script>

<template>
  <header>
    <!-- Logo Vue.js -->
    <img alt="Vue logo" class="logo" height="125" src="@/assets/logo.svg" width="125"/>

    <div class="wrapper">
      <!-- Afficher un message de bienvenue avec le composant HelloWorld -->
      <HelloWorld msg="You did it!"/>

      <!-- Navigation pour les différentes pages -->
      <nav>
        <!-- Les liens de navigation pour les différentes pages -->
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/categories">Catégories</RouterLink>
        <RouterLink to="/product">Product</RouterLink>
      </nav>
    </div>
  </header>

  <!-- Affiche le composant de la route actuelle -->
  <RouterView/>
</template>

<style scoped>
/* Styles pour le header et le logo */
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

/* Styles pour la navigation */
nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

/* Styles pour les liens actifs */
nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

/* Styles généraux pour les liens de navigation */
nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

/* Enlever la bordure gauche du premier lien */
nav a:first-of-type {
  border: 0;
}

/* Styles pour les grands écrans */
@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;
    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
