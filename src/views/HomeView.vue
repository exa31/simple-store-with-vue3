<script setup lang="ts">
import ProductCard from '@/components/ProductCard.vue'
import Pagination from '@/components/Pagination.vue'
import { computed, onBeforeMount, ref, watch, watchEffect } from 'vue'
import axios from 'axios'

const products = ref<{
  _id: string
  title: string
  description: string
  price: number
  image_url: string
}[]>([])
const totalProducts = ref(0)
const loading = ref(true)
const currentPage = ref(1)
const totalPages = computed(() => Math.ceil(totalProducts.value / 12))

onBeforeMount(async () => {
  try {
    const response = await axios.get(`${import.meta.env.VITE_API_PRODUCTS_URL}/products?limit=12&skip=${(currentPage.value - 1) * 12}`)
    products.value = await response.data.products
    totalProducts.value = await response.data.data
  } catch (error) {
    console.error(error)
  } finally {
    loading.value = false
  }
})

watch(
  () => currentPage.value,
  async () => {
    try {
      loading.value = true
      const response = await axios.get(`${import.meta.env.VITE_API_PRODUCTS_URL}/products?limit=12&skip=${(currentPage.value - 1) * 12}`)
      products.value = await response.data.products
    } catch (error) {
      console.error(error)
    } finally {
      loading.value = false
    }

  }
)

const updatePage = (newPage: number) => {
  currentPage.value = newPage
}

</script>



<template>
  <div v-if="loading">
    Loading...
  </div>
  <main v-else>
    <div class="product-grid">
      <!-- Product 1 -->
      <ProductCard v-for="product in products" :product="product" :key="product._id" />
    </div>
    <Pagination :currentPage="currentPage" :totalPages="totalPages" @updatePage="updatePage" />
  </main>
</template>




<style scoped>
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  width: 80%;
  margin: 0 auto;
}
</style>
