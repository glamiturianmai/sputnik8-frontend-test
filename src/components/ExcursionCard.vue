<template>
  <div class="excursion-card">
    <h3>{{ excursion.title }}</h3>
    <p><strong>Цена:</strong> {{ excursion.netto_price }}</p>
    <p><strong>Рейтинг:</strong> {{ excursion.customers_review_rating }}</p>
    <p><strong>Кол-во оценок:</strong> {{ excursion.reviews }}</p>
   <img :src="excursion.main_photo.original" alt="img" class="excursion-image" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    excursion: {
      type: Object as () => {
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
      },
      required: true
    }
  }
});
</script>

<style scoped>
.excursion-card {
  
}

.excursion-image {
  max-width: 100px; 
  min-height: 100px; 
 
}
</style>
