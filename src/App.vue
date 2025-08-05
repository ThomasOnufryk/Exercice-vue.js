<template>
  <div class="block">
    <div class="list">
      <h1>Mes Articles</h1>
      <template v-if="articles.length">
        <div class="articles-list">
          <ul>
            <li v-for="article in articles" @click="setCurrentArticle(article)" :key="article.id">
              <ArticleList :class="{ selected: currentIndex === article.id }" :article="article" />
            </li>
          </ul>
        </div>
      </template>
      <template v-else>
        <h3>Vous n'avez pas encore enregistré d'articles dans votre liste</h3>
      </template>
    </div>

    <ArticleForm v-model:article="currentArticle" v-model:editionMode="editionMode" @save-article="saveArticle"
      @new-article="newArticle" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import ArticleForm from "@/components/ArticleForm.vue";
import ArticleList from "@/components/ArticleList.vue";

const articles = ref([
  { id: 1, name: "Achat de matériel", price: 20.0, vat: 20.0 },
  { id: 2, name: "Dépose des murs", price: 150.0, vat: 20.0 },
]);

const editionMode = ref(false);
const currentArticle = ref({});
const currentIndex = ref(-1);

const setCurrentArticle = (article) => {
  currentArticle.value = { ...article };
  currentIndex.value = article.id;
  editionMode.value = true;
};

const saveArticle = () => {
  if (editionMode.value && currentIndex.value >= 0) {
    const articleIndex = articles.value.findIndex(
      (a) => a.id === currentIndex.value
    );
    articles.value[articleIndex] = { ...currentArticle.value };
    console.log(articles.value);
  } else {
    const articleId = articles.value.length
      ? Math.max(...articles.value.map((a) => a.id)) + 1
      : 1;
    articles.value.push({ id: articleId, ...currentArticle.value });
    console.log(articles.value);
  }

  editionMode.value = false;
  currentArticle.value = {};
  currentIndex.value = -1;
};

const newArticle = () => {
  editionMode.value = false;
  currentArticle.value = {};
  currentIndex.value = -1;
};
</script>

<style scoped>
.selected {
  background-color: lightgrey;
}
</style>
