<script setup>
import CardItem from '@/components/CardItem.vue';
import {onMounted, reactive, ref, watch} from 'vue';
import PaginationBar from '@/components/PaginationBar.vue';
import FilterBar from '@/components/FilterBar.vue';

let state = reactive({
  allData: [],
  fetchedData: [],
  filteredData: [],
  selectedStatuses: [],
  currentPage: 1,
  totalPages: 1,
  showAllButtons: false,
  isMobileView: false,
});

const fetchData = async () => {
  try {
    const response = await fetch(`https://rickandmortyapi.com/api/character`);
    const data = await response.json();
    state.totalPages = data.info.pages;

    for (let page = 1; page <= state.totalPages; page++) {
      const pageResponse = await fetch(`https://rickandmortyapi.com/api/character?page=${page}`);
      const pageData = await pageResponse.json();
      state.allData.push(...pageData.results);
    }

    state.filteredData = [...state.allData];
    updatePageData();
  } catch (error) {
    console.error(error);
  }
};

const updatePageData = () => {
  const start = (state.currentPage - 1) * 20;
  const end = start + 20;
  state.fetchedData = state.filteredData.slice(start, end);
};

fetchData();

function checkScreenSize() {
  state.isMobileView = window.innerWidth <= 600;
}

onMounted(() => {
  window.addEventListener('resize', checkScreenSize);
  checkScreenSize();
});

watch(() => state.currentPage, updatePageData);
watch(() => state.filteredData, updatePageData);
watch(() => state.selectedStatuses, updatePageData);
</script>

<template>
  <header>
    <h1 class="title">Rick and Morty<br></h1>
    <h2>characters</h2>
  </header>
  <main>
    <PaginationBar :state="state"/>
    <FilterBar :state="state"/>
    <p v-show="state.filteredData.length === 0">There's nothing here...</p>
    <ul class="cards-list">
      <CardItem v-for="character in state.fetchedData" :key="character.id" :character="character" />
    </ul>
  </main>
</template>

<style scoped>

  p {
    text-align: center;
  }

  header {
    text-align: center;
    font-family: "Get Schwifty", sans-serif;
    height: 23vh;
  }

 .cards-list {
   max-width: 1280px;
   margin: 0 auto;
   list-style: none;
   display: grid;
   grid-template-columns: repeat(4, 1fr);
   grid-column-gap: 0px;
   grid-row-gap: 2em;
 }

  .title {
    font-size: 4.5vw;
    color: #b3ccb0;
  }

  h2 {
    font-size: 1.8vw;
  }

 @media screen and (max-width: 1150px) {
   .cards-list {
     grid-template-columns: repeat(3, 1fr);
     grid-column-gap: 0px;
     grid-row-gap: 2em;
   }

   .title {
     font-size: 8vw;
   }

   h2 {
     font-size: 3.5vw;
   }
 }

  @media screen and (max-width: 900px) {
    .cards-list {
      grid-template-columns: repeat(2, 1fr);
      grid-column-gap: 0px;
      grid-row-gap: 2em;
    }
  }

 @media screen and (max-width: 650px) {
   .cards-list {
     list-style: none;
     display: flex;
     flex-direction: column;
     justify-content: center;
     align-items: center;
     padding: 0;
   }

   .title {
     font-size: 13vw;
   }

   h2 {
     font-size: 6vw;
   }
 }
</style>
