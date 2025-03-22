<template class="form">
  <h1>{{ title }}</h1>
  <div class="form__inputs">

    <input type="text" name="articleName" id="articleName" v-model="article.name" placeholder="Nom de article"/>

    <div class="form-label">
      <label for="articlePrice">
        Prix unitaire HT
      </label>
      <input type="text" name="articlePrice" id="articlePrice" v-model="article.price" placeholder="20"/>
    </div>

    <div class="form-label">
      <label for="vat">
        TVA en %
      </label>
      <input type="text" name="vat" id="vat" v-model="article.vat" placeholder="20%">
    </div>
    <div class="total">
      <div>Prix total TTC</div>
      <div>{{ priceTi }} €</div>
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
  return !isNaN(article.value.price) ? (article.value.price * (1 + article.value.vat / 100)).toFixed(2) : 0;
})
</script>