<script setup>
import { onMounted, reactive } from "vue";
import { doAjaxRequest } from "@/api";

// Variable pour suivre la page actuelle
let currentPage = 0;

// Objet réactif pour stocker les données des produits et le nombre total de pages
const state = reactive({
  products: [],
  totalPages: 0
});

// Fonction pour récupérer les produits depuis l'API
const fetchProducts = () => {
  const url = `/api/produits?page=${currentPage}&size=5&sort=nom,asc`;
  const options = { method: "GET" };

  doAjaxRequest(url, options)
      .then((response) => {
        state.products = response._embedded.produits;
        state.totalPages = response.page.totalPages;
      })
      .catch((error) => {
        console.error("Erreur lors de la récupération des produits:", error);
      });
};

// Fonction pour aller à la première page
const goToFirstPage = () => {
  if (currentPage !== 0) {
    currentPage = 0;
    fetchProducts();
  }
};

// Fonction pour aller à la dernière page
const goToLastPage = () => {
  if (currentPage !== state.totalPages - 1) {
    currentPage = state.totalPages - 1;
    fetchProducts();
  }
};

// Fonction pour aller à la page suivante
const goToNextPage = () => {
  if (currentPage < state.totalPages - 1) {
    currentPage++;
    fetchProducts();
  }
};

// Fonction pour aller à la page précédente
const goToPreviousPage = () => {
  if (currentPage > 0) {
    currentPage--;
    fetchProducts();
  }
};

// Appel initial pour charger les produits lors du montage du composant
onMounted(fetchProducts);
</script>

<template>
  <div>
    <table>
      <caption>Les produits - page {{ currentPage + 1 }} / {{ state.totalPages }}</caption>
      <thead>
      <tr>
        <th>Nom</th>
        <th>Prix</th>
        <th>Stock</th>
        <th>Commandés</th>
      </tr>
      </thead>
      <tbody>
      <tr v-if="state.products.length === 0">
        <td colspan="4">Veuillez patienter, chargement des produits...</td>
      </tr>
      <tr v-for="product in state.products" :key="product.reference">
        <td>{{ product.nom }}</td>
        <td>{{ product.prixUnitaire }}</td>
        <td>{{ product.unitesEnStock }}</td>
        <td>{{ product.unitesCommandees }}</td>
      </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button @click="goToFirstPage" :disabled="currentPage === 0">⇆</button>
      <button @click="goToPreviousPage" :disabled="currentPage === 0">←</button>
      <button @click="goToNextPage" :disabled="currentPage >= state.totalPages - 1">→</button>
      <button @click="goToLastPage" :disabled="currentPage >= state.totalPages - 1">⇄</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProduitView"
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
}

th, td {
  border: 1px solid black;
  padding: 8px;
}

th {
  background-color: #232623;
  color: white;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination button {
  margin: 0 5px;
  padding: 10px;
  border: none;
  background-color: #232623;
  color: white;
  cursor: pointer;
}

.pagination button:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}
</style>
