<template>
    <div class="pagination">
        <button @click="prevPage" :disabled="currentPage === 1">Anterior</button>
        <span>Página {{ currentPage }} de {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Siguiente</button>
    </div>
</template>

<script setup>
const props = defineProps({
    currentPage: Number,
    totalPages: Number,
});

const emit = defineEmits(['changePage']);

const prevPage = () => {
    if (props.currentPage > 1) {
        emit('changePage', props.currentPage - 1);
    }
};

const nextPage = () => {
    if (props.currentPage < props.totalPages) {
        emit('changePage', props.currentPage + 1);
    }
};
</script>

<style scoped>
.pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

button {
    margin: 0 10px;
}
</style>