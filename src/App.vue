<template>
  <div id="app">
    <h1>Экскурсии по всему миру</h1>
    <div class="filters">
      <div class="input-container">
        <input
          type="text"
          placeholder="Введите название экскурсии"
          v-model="excursionFilter"
        />
        <span class="reset-button" @click="clearExcursionFilter" v-if="excursionFilter">✖</span>
      </div>
      <select v-model="selectedCityId">
        <option value="" disabled selected>Выбрать город</option>
        <option v-for="city in cities" :key="city.id" :value="city.id">{{ city.name }}</option>
      </select>
      <button @click="filterExcursions">Filter</button>
    </div>
    <div class="excursion-list">
      <ExcursionCard
        v-for="excursion in displayedExcursions"
        :key="excursion.id"
        :excursion="excursion"
      />
    </div>
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    <button v-if="errorMessage" @click="resetFilters">Сбросить фильтры</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue';
import axios from 'axios';
import ExcursionCard from './components/ExcursionCard.vue';

interface City {
  id: number;
  name: string;
}

interface Excursion {
  id: number;
  title: string;
  main_photo: 
    {
      original: string;
    };
  customers_review_rating: number;
  reviews: number;
  city_id: number;
  netto_price: string;
}

export default defineComponent({
  components: {
    ExcursionCard,
  },
  setup() {
    const cities = ref<City[]>([]);
    const excursions = ref<Excursion[]>([]);
    const excursionFilter = ref('');
    const selectedCityId = ref<number | null>(null);
    const errorMessage = ref('');

    const fetchData = async () => {
      try {
        const citiesResponse = await axios.get<City[]>('https://cors-anywhere.herokuapp.com/https://api.sputnik8.com/v1/cities?api_key=873fa71c061b0c36d9ad7e47ec3635d9&username=frontend@sputnik8.com');
        cities.value = citiesResponse.data;

        const excursionsResponse = await axios.get<Excursion[]>('https://cors-anywhere.herokuapp.com/https://api.sputnik8.com/v1/products?api_key=873fa71c061b0c36d9ad7e47ec3635d9&username=frontend@sputnik8.com');
        excursions.value = excursionsResponse.data;
      } catch (error) {
        errorMessage.value = 'Error fetch';
      }
    };

    const displayedExcursions = computed(() => {
      if (!selectedCityId.value && !excursionFilter.value) {
        errorMessage.value = ''; 
        return excursions.value; 
      }

      const filtered = excursions.value.filter(excursion => {
        const matchesExcursionName = excursion.title.toLowerCase().includes(excursionFilter.value.toLowerCase());
        const matchesCityId = selectedCityId.value ? excursion.city_id === selectedCityId.value : true;
        return matchesExcursionName && matchesCityId;
      });

      errorMessage.value = filtered.length === 0 ? 'Поиск не дал результатов' : '';
      return filtered;
    });

    const clearExcursionFilter = () => {
      excursionFilter.value = '';
    };

    const resetFilters = () => {
      excursionFilter.value = '';
      selectedCityId.value = null;
      errorMessage.value = ''; 
    };

    onMounted(fetchData); 

    return {
      excursionFilter,
      selectedCityId,
      displayedExcursions,
      cities,
      errorMessage,
      clearExcursionFilter,
      resetFilters,
    };
  },
});
</script>

<style scoped>
.filters {
  margin-bottom: 20px;
}

.input-container {
  position: relative;
  display: inline-block;
}

.clear-button {
  position: absolute;
  cursor: pointer;
}

.excursion-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.error-message {
  color: red;
}
</style>
