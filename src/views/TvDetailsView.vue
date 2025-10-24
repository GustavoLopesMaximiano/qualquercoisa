<script setup>
  import { defineProps, onMounted } from 'vue';
  import { useTvStore } from '@/stores/tv';
  const tvStore = useTvStore();

  const props = defineProps({
    tvId: {
      type: Number,
      required: true,
    },
  });

  onMounted(async () => {
    await tvStore.getTvDetail(props.tvId);
  });
  const formatDate = (date) => new Date(date).toLocaleDateString('pt-BR');
</script>

<template>
  <div class="main">
    <div class="content">
      <div class="image">
        <img
        :src="`https://image.tmdb.org/t/p/w185${tvStore.currentTv.poster_path}`"
        :alt="tvStore.currentTv.title"
      />
      </div>
      <div class="details">
        <h1>{{ tvStore.currentTv.name }}</h1>
        <p>{{ tvStore.currentTv.tagline }}</p>
        <p>{{ tvStore.currentTv.overview }}</p>
        <p>Primeiro episódio: {{ formatDate(tvStore.currentTv.first_air_date) }}</p>
        <p>Último episódio: {{ formatDate(tvStore.currentTv.last_air_date) }}</p>
        <p>Avaliação: {{ tvStore.currentTv.vote_average }}</p>
        
      <div class="companies">
        <p>Produtoras:</p>
      <template
      v-for="company in tvStore.currentTv.production_companies"
      :key="company.id">
      
      <img
        v-if="company.logo_path"
        :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`"
        :alt="company.name"
      />
      <p v-else>{{ company.name }}</p>
    </template>
  </div>
      </div>
    </div>
  </div>

  
</template>

<style scoped>
.content {
    display: flex;
    margin: 2rem;
    gap: 2rem;
  }
  .companies {
    display: flex;
    flex-direction: row;
    column-gap: 3rem;
    align-items: center;
    margin-bottom: 2rem;
  }
  .companies img {
    max-height: 2rem;
  }
  .image img {
    border-radius: 8px;
    height: 30rem;
  }
  .details {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
</style>