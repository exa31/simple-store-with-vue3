<script setup lang="ts">
import { defineProps } from 'vue'
import { formatRupiah } from '@/utils'
import { useRouter } from 'vue-router'


const route = useRouter()

const props = defineProps<{
    product: {
        _id: string
        title: string
        description: string
        price: number
        image_url: string
    }
}>()

const { product } = props

</script>

<template>
    <div class="product-card" @click="route.push(`/products/${product._id}`)">
        <img :src="product.image_url" :alt="product.title" class="product-image" />
        <h2 class="product-title">{{ product.title }}</h2>
        <p class="product-description line-clamp-3">{{ product.description }}</p>
        <span class="product-price">{{ formatRupiah(product.price) }}</span>
    </div>
</template>

<style scoped>
.product-card {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease-in-out;
    cursor: pointer;
}

.product-card:hover {
    transform: scale(1.05);
}

.product-image {
    width: 100%;
    height: 150px;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 10px;
}

.product-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.product-description {
    color: #555;
    margin-bottom: 10px;
}

.product-price {
    color: #00b894;
    font-weight: bold;
}
</style>