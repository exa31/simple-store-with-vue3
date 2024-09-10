<script setup lang="ts">
const props = defineProps<{
    currentPage: number
    totalPages: number
}>()
const emit = defineEmits(['updatePage'])

const prevPage = () => {
    emit('updatePage', props.currentPage - 1)
}

const nextPage = () => {
    emit('updatePage', props.currentPage + 1)
}

</script>

<template>
    <div class="pagination">
        <button @click="prevPage" v-if="currentPage !== 1" class="pagination-button" id="prevPage">&lt;</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" v-if="currentPage !== totalPages" class="pagination-button"
            id="nextPage">&gt;</button>
    </div>
</template>


<style scoped>
.pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

.pagination-button {
    background-color: #fff;
    color: #00b894;
    padding: 8px 12px;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    font-size: 16px;
    margin: 0 5px;
}
</style>