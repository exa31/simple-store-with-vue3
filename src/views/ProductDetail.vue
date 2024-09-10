<script setup lang="ts">
import { onBeforeMount, ref } from 'vue';
import { formatRupiah } from '../helper/index';
import { useRoute } from 'vue-router';
import axios from 'axios';

const route = useRoute();

const product = ref<{
    _id: string
    title: string
    description: string
    price: number
    image_url: string
}>();
// console.log(product.value.product._id)
onBeforeMount(() => {
    fetchData();
});
async function fetchData() {
    const API_URL = `${import.meta.env.VITE_API_PRODUCTS_URL}/products/${route.params.id}`;
    try {
        const response = await axios.get(API_URL);
        console.log(response.data)
        product.value = response.data;
    } catch (error) {
        console.error(error);
    }
}
</script>

<template>
    <div class="product-detail">
        <h2>{{ product?.title }}</h2>
        <img :src="product?.image_url" :alt="product?.title" class="product-image" />
        <p>Description: {{ product?.description }}</p>
        <p v-if="product?.price">Price: {{ formatRupiah(product?.price) }}</p>
        <router-link to="/" class="back-button">Back</router-link>
    </div>
</template>

<style scoped>
/* Styling untuk halaman detail produk */
.product-detail {
    margin-top: 20px;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
    text-align: center;
}

.product-image {
    max-width: 100%;
    height: auto;
    margin: 0 auto;
    margin-bottom: 10px;
}

.product-detail p {
    margin-bottom: 5px;
}

.product-detail button {
    margin-top: 10px;
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.back-button {
    display: inline-block;
    padding: 8px 16px;
    background-color: #007bff;
    color: #fff;
    text-decoration: none;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.back-button:hover {
    background-color: #0056b3;
}
</style>