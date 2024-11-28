<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean,
})
const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :on-click-favorite="() => (isFavorites ? null : emit('addToFavorite', item))"
      :on-click-add="() => (isFavorites ? null : emit('addToCart', item))"
      :is-added="item.isAdded"
      :is-favorite="item.isFavorite"
    />
  </div>
</template>
