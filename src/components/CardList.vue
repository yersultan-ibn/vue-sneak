<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
const onClickAdd = () => {
  alert('Добавить')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      v-bind:key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isFavorite="false"
      :isAdded="false"
      :onClickFavorite="() => {}"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
