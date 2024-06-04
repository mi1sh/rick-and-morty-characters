<script setup>
import {ref} from 'vue';

const props = defineProps({
  state: Object,
});

const nameInput = ref("");
const statusOptions = ["Alive", "Dead", "Unknown"];

const filterData = () => {
  let filteredData = [...props.state.allData];

  if (nameInput.value.trim() !== "") {
    filteredData = filteredData.filter(item =>
        item.name.toLowerCase().includes(nameInput.value.toLowerCase())
    );
  }

  if (props.state.selectedStatuses.length > 0) {
    filteredData = filteredData.filter(item =>
        props.state.selectedStatuses.includes(item.status.charAt(0).toUpperCase() + item.status.slice(1))
    );
  }

  props.state.filteredData = filteredData;
  props.state.currentPage = 1;
  updateTotalPages();
};

const updateTotalPages = () => {
  props.state.totalPages = Math.ceil(props.state.filteredData.length / 20);
};

</script>

<template>
  <section class="filter-bar">
    <div class="filters-wrapper">
      <input placeholder="Search by name..." v-model="nameInput" type="text" />
      <div class="status-wrapper">
        <span>Status:</span>
        <div class="status-bar">
          <label v-for="status in statusOptions" :key="status">
            <input
                type="checkbox"
                :value="status"
                v-model="props.state.selectedStatuses"
            />
            {{ status }}
          </label>
        </div>
      </div>
    </div>
    <button @click="filterData">Search</button>
  </section>
</template>

<style scoped>
.filter-bar {
  text-align: center;
  margin-bottom: 2em;
}


.status-bar {
  display: flex;
  flex-direction: column;
  text-align: left;
}

.filters-wrapper {
  display: flex;
  justify-content: center;
  gap: 1em;
  align-items: center;
}

.status-wrapper {
  display: flex;
  align-items: center;
  gap: 0.5em;
}
@media (max-width: 800px) {
  .filters-wrapper {
    flex-direction: column;
  }
}
</style>