<script setup>
const props = defineProps({
  character: Object,
});
</script>

<template>
  <div class="card">
    <img :src="props.character.image" :alt="props.character.name">
    <div class="card-info">
      <h2>{{ props.character.name }}</h2>
      <p>Status - {{ props.character.status }}</p>
      <p>Last seen: {{ props.character.location.name }}</p>
      <p>First seen: {{ props.character.origin.name }}</p>
    </div>
  </div>
</template>

<style scoped>
  .card {
    font-size: 0.8em;
    background-color: #1f231f;
    width: 20em;
    border-radius: 2em;

    &:hover {
      transition: 0.2s ease-in-out;
      transform: scale(1.03);
    }
  }

  .card-info {
    padding: 5px 0px 15px 10px;
  }

  img {
    width: 100%;
    border-radius: 2em;
  }

  h2 {
    font-weight: bold;
  }

  @media screen and (max-width: 650px) {
    .card {
      width: 70vw;
    }
  }
</style>