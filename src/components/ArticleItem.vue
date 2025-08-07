<template>
  <div class="article-group">
    <p class="list-item">{{ article.name }}</p>
    <div class="arrow">
    </div>
  </div>
</template>

<script setup>
defineProps({
  article: {
    type: Object,
    required: false,
  }
});
</script>

