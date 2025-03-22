<template>
  <div class="block">
    <div class="list">
      <h1>Mes Articles</h1>
      <ul>
        <li v-for="(article, index) in articles" @click="setCurrentArticle(article)" :key="index">
          <ArticleList :article="article"/>
        </li>
      </ul>
    </div>
    <div class="form">
      <ArticleForm v-model:article="currentArticle" v-model:editionMode="editionMode"/>
    </div>
  </div>

</template>

<script setup>
import { ref} from "vue";
import ArticleForm from "@/components/ArticleForm.vue";
import ArticleList from "@/components/ArticleList.vue";
const articles = ref([
  { name:"Achat de matériel", price: 20, vat:20},
  { name:"Dépose des murs", price: 150, vat:20},
])
const editionMode = ref(false);
const currentArticle = ref({})
const setCurrentArticle = (article) => {
  currentArticle.value = article;
  editionMode.value = true;
  }


</script>

<style scoped>

</style>
