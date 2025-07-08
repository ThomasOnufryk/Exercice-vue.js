<template>
  <div class="article-group">
    <p class="list-item">{{ article.name }}</p>
    <div class="arrow">
    </div>
  </div>
</template>

<script setup>

const props = defineProps({
  article: Object,
})

</script>

