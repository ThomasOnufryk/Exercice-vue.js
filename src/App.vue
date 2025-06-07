<template>
  <div class="block">
    <div class="list">
      <h1>Mes Articles</h1>
      <template v-if="articles.length">
        <div class="ul">
          <ul>
            <li v-for="(article, index) in articles" @click="setCurrentArticle(article, index)" :key="index">
              <ArticleList :article="article"/>
            </li>
          </ul>

        </div>
      </template>
      <template v-else>
        <h3>Vous n'avez pas encore enregistré d'articles dans votre liste</h3>
      </template>
    </div>

    <ArticleForm v-model:article="currentArticle" v-model:editionMode="editionMode" @save-article="saveArticle"/>

  </div>
</template>

<script setup>
import {ref} from "vue";
import ArticleForm from "@/components/ArticleForm.vue";
import ArticleList from "@/components/ArticleList.vue";

const articles = ref([
  {name: "Achat de matériel", price: 20.00, vat: 20.00},
  {name: "Dépose des murs", price: 150.00, vat: 20.00},
])
const editionMode = ref(false);
const currentArticle = ref({})
const currentIndex = ref(-1); // Pour tracker l'index de l'article en cours d'édition

const setCurrentArticle = (article, index) => {
  // Créer une copie de l'article pour éviter de modifier l'original
  currentArticle.value = {...article};
  currentIndex.value = index;
  editionMode.value = true;
}

const saveArticle = () => {
  if (editionMode.value && currentIndex.value >= 0) {
    // Mode édition : remplacer l'article existant
    articles.value[currentIndex.value] = {...currentArticle.value};
  } else {
    // Mode création : ajouter un nouvel article
    articles.value.push({...currentArticle.value});
  }

  // Réinitialiser après sauvegarde
  editionMode.value = false;
  currentArticle.value = {};
  currentIndex.value = -1;
}

const newArticle = () => {
  editionMode.value = false;
  currentArticle.value = {};
  currentIndex.value = -1;
}
</script>

<style scoped>

</style>
