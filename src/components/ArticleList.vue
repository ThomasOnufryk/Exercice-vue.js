<template>
  <p class="list-item">{{ article.name }}</p>
</template>

<script setup>

const props = defineProps({
  article: Object,
})
</script>