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
              type="number"
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
              type="number"
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

      <button class="submit" type="button" @click="handleSave">{{ buttonTitle }}</button>
    </form>
  </div>
</template>

<script setup>
import {computed} from "vue";

const edition = defineModel('editionMode')
const article = defineModel('article')

// Définir les émissions possibles
const emit = defineEmits(['save-article'])

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

}function formatVat(event) {
  const formattedVat = parseFloat(article.value.price) || 0;
  event.target.value = formattedVat.toFixed(2);
}


const handleSave = () => {
  // Validation basique avant émission
  console.log("ARTICLE", article.value)
  if (!article.value.name || !article.value.price) {
    alert('Veuillez remplir au minimum le nom et le prix de l\'article');
    return;
  }

  emit('save-article');
}
</script>