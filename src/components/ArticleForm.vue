<template class="form">
  <h1>{{ title }}</h1>
  <div class="form">
    <label for="articleName">
      <input type="text" name="articleName" id="articleName" v-model="article.name" placeholder="Nom de article" />
    </label>
    <label for="articlePrice">
      Prix unitaire HT
    <input type="text" name="articlePrice" id="articlePrice" v-model="article.price" placeholder="20" />
    </label>
    <label for="vat">
      TVA en %
    <input type="text" name="vat" id="vat" v-model="article.vat" placeholder="20%">
    </label>
    <div class="total">
      <div>Prix total TTC</div>
      <div>{{priceTi}} €</div>
    </div>
    <button class="submit">{{ buttonTitle }}</button>
  </div>
</template>

<script setup>
import {computed} from "vue";

const edition = defineModel('editionMode')
const article = defineModel('article')
const title = computed(() => {
  return edition.value === true ? "Editer un article" : "Ajouter un article";
    }
)
const buttonTitle = computed(() => {
  return edition.value === true ? "Enregistrer" : "Ajouter";
    }
)
const priceTi = computed(() => {
  return (article.value.price * 1+(article.value.vat/100)).toFixed(2);
})
</script>