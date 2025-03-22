<template>
 <p>{{article.name}}</p>
</template>

<script setup>

const props = defineProps({
  article: Object,
})
</script>