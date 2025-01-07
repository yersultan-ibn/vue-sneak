<script setup>
import { ref, onMounted, watch, nextTick } from 'vue'
import axios from 'axios'
import CardLIst from './components/CardList.vue'
import Header from './components/Header.vue'

const items = ref([])

const filters = ref({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://604781a0efa572c1.mokky.dev/favorites`)
    items.value = favorites.map((item) => {
      const favorite = favorites.find((favorite) => favorite.productId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.error('Ошибка при загрузке данных:', error)
  }
}

const fetchItems = async () => {
  const params = {
    sortBy: filters.sortBy,
    searchQuery: filters.searchQuery
  }

  if (filters.searchQuery) {
    params.title = `*${filters.searchQuery}*`
  }

  try {
    const { data } = await axios.get(`https://604781a0efa572c1.mokky.dev/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.error('Ошибка при загрузке данных:', error)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)
</script>

<template>
  <div class="bg-white w-4/5 m-auto h-screen rounded-xl shadow-xl mt-20">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center mb-10">
        <h1 class="text-3xl font-bold">Все кроссовки</h1>
        <div class="flex items-center gap-4">
          <select
            @change="onChangeSelect"
            class="py-2 px-3 border border-gray-200 focus:border-gray-400 rounded-md focus:outline-none"
          >
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <input
              @change="onChangeSearchInput"
              type="text"
              class="border border-gray-200 rounded-md py-2 pl-10 pr-4 focus:outline-none focus:border-gray-400"
              placeholder="Поиск..."
            />
            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
              <img src="/search.svg" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <CardLIst :items="items" />
  </div>
</template>
