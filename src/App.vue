<template>
  <div id="app">
    <div class="head-contaner">
      <img src="./logo.png" alt="img" class="logo-image" />
      <h1>Экскурсии по всему миру</h1>
      <div class="input-container">
          <input
          
            type="text"
            placeholder="Введите название экскурсии"
            v-model="excursionFilter"
            class="excursion-input"
          />
          <span class="reset-button" @click="clearExcursionFilter" v-if="excursionFilter">✖</span>
        <select v-model="selectedCityId">
          <option value="" disabled selected>Выбрать город</option>
          <option v-for="city in cities" :key="city.id" :value="city.id">{{ city.name }}</option>
        </select>
      </div>
    </div>
    <div class="excursion-container">
      <ExcursionCard
        v-for="excursion in displayedExcursions"
        :key="excursion.id"
        :excursion="excursion"
      />
    </div>
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    <button v-if="errorMessage" class="error-button" @click="resetFilters">Сбросить фильтры</button>
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

.input-container {
  position: relative;  
  display: inline-block;  
}


.reset-button {
  position: absolute; 
  left: 300px; 
  top: 50%; 
  transform: translateY(-50%);  
  cursor: pointer; 
  font-size: 16px;
  color: #888; 
  user-select: none; 
}


input, select{
  border-radius: 1px;
  border-color:  #999999;
  width: 300px;
  height: 50px;
  color: #999999;
  font-size: 16px;
  width: 400;
  margin: auto 15px;
  text-align: center;
}
h1{
  font-size: 48px;
  font-weight: 700;
  color: #000000;
  margin: 30px auto 50px auto;

}

.head-contaner{
  display: flex;
  flex-direction: column;
  align-items: center;

}

.input-container{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: stretch;

}
.excursion-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 50px;
  margin-top: 90px;
}

.logo-image {
  margin-top: 50px;
}

.error-button{
  background-color: #00A7FF;
  color: #FFFFFF;
  font-size: 14px;
  font-weight: 400;
  border-radius: 3px;
  width: 199px;
  height: 40px;
  border: 0;

}
.error-message {
  font-size: 24px;
  font-weight: 400;
  color: #000000;
}
</style>
