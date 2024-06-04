<script setup>
import {onMounted, ref} from 'vue';

const props = defineProps({
  state: Object,
});

let showPaginationButtons = ref(true);

onMounted(() => {
  function handleResize() {
    showPaginationButtons.value = window.innerWidth > 1510;
  }

  window.addEventListener('resize', handleResize);
  handleResize();

  return () => {
    window.removeEventListener('resize', handleResize);
  };
});

const toggleShowAllButtons = () => {
  props.state.showAllButtons = !props.state.showAllButtons;
  showPaginationButtons.value = !!props.state.showAllButtons;
};
</script>

<template>
  <section class="pagination-bar">
    <button @click="props.state.currentPage--" :disabled="props.state.currentPage === 1">&lt;</button>
    <button class="show-all-btn" @click="toggleShowAllButtons">...</button>
    <div v-show="props.state.showAllButtons || showPaginationButtons">
      <button
          class="pagination-button"
          v-for="page in Math.ceil(props.state.filteredData.length / 20)"
          :key="page"
          @click="props.state.currentPage = page"
          :class="{ active: props.state.currentPage === page }"
      >
        {{ page }}
      </button>
    </div>
    <button @click="props.state.currentPage++"
            :disabled="props.state.currentPage === Math.ceil(props.state.filteredData.length / 20)">&gt;
    </button>
  </section>
</template>

<style scoped>
.pagination-bar {
  justify-content: center;
  display: flex;
  margin-bottom: 1em;
}

.show-all-btn {
  display: none;
}

@media (min-width: 601px) {
  .show-all-btn {
    display: none;
  }
}

@media (max-width: 1510px) {
  .show-all-btn {
    display: block;
  }

  .pagination-button {
    font-size: 1.5vw;
  }
}

@media (max-width: 800px) {
  .show-all-btn {
    display: block;
  }

  .pagination-button {
    font-size: 3.5vw;
  }
}
</style>