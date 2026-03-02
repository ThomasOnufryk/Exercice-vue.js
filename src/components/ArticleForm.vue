<template>
  <div class="form">
    <h1>{{ title }}</h1>
    <form class="form__inputs">

      <input type="text" name="articleName" id="articleName" v-model="article.name" placeholder="Nom de l'article"/>

      <div class="form-label">
        <label for="articlePrice">
          Prix unitaire HT
        </label>
        <div class="input-group">
          <input
              name="articlePrice"
              id="articlePrice"
              v-model.number="article.price"
              @blur="formatPrice"
              inputmode="decimal"
              placeholder="20.00"
              min="0"
          />
          <span class="input-suffix">€</span>
        </div>
      </div>

      <div class="form-label">
        <label for="vat">
          TVA
        </label>
        <div class="input-group">
          <input
              name="vat"
              id="vat"
              v-model.number="article.vat"
              placeholder="20,00"
              min="0"
              max="100"
              inputmode="decimal"
              @blur="formatVat"
          />
          <span class="input-suffix">%</span>
        </div>
      </div>

      <div class="total">
        <div>Prix total TTC</div>
        <div>{{ priceTi }} €</div>
      </div>

      <button class="button" :disabled="!disableButton" type="button" @click="handleSave">{{ buttonTitle }}</button>
      <button class="button button--white" v-show="edition" type="button" @click="handleNewArticle">Nouvel article</button>
    </form>
  </div>
</template>

<script setup>
import {computed} from "vue";

const edition = defineModel('editionMode', {
  type: Boolean,
  default: false
})
const article = defineModel('article', {
  type: Object,
  required: false,
})

// Définir les émissions possibles
const emit = defineEmits(['save-article', 'new-article'])

const title = computed(() => {
  return edition.value === true ? "Editer un article" : "Ajouter un article";
})

const buttonTitle = computed(() => {
  return edition.value === true ? "Enregistrer" : "Ajouter";
})

const priceTi = computed(() => {
  const price = parseFloat(article.value.price) || 0;
  const vat = parseFloat(article.value.vat) || 0;
  return (price * (1 + vat / 100)).toFixed(2);
})

function formatPrice(event) {
  const formattedPrice = parseFloat(article.value.price) || 0;
  event.target.value = formattedPrice.toFixed(2);
}

function formatVat(event) {
  const formattedVat = parseFloat(article.value.vat) || 0;
  event.target.value = formattedVat.toFixed(2);
}

function checkNumber(value) {
  if (value === undefined) return true;
  return !isNaN(value);
}

const disableButton = computed(() => {
  return !(!checkNumber(article.value.price) || !checkNumber(article.value.vat));
})

const handleNewArticle = () => {
  emit("new-article");
}

const handleSave = () => {

  if (!article.value.name || !article.value.price) {
    alert("Veuillez remplir au minimum le nom et le prix de l'article");
    return;
  }
  if (!checkNumber(article.value.price) || !checkNumber(article.value.vat)) {
    alert("Merci de n'indiquer que des valeurs numériques")
    return;
  }

  emit('save-article');
}
</script>