<template>
  <div class="excursion-card">
    <img :src="excursion.main_photo.original" alt="img" class="excursion-image" />
    <div class="rank-container">
        <img :src={star} alt="img" class="star-image" />
        <p class="rank"> {{ excursion.customers_review_rating }}</p>
        <p class="inf">  {{ excursion.reviews }}</p>
    </div>
    <div class="name">{{ excursion.title }}</div>
    <div class="price-container">
        <p class="price"> от {{ excursion.netto_price }} Р</p>
        <p class="inf"> за экскурсию</p>
    </div>
   
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import {star} from "./../assets/star.png"
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
    display: flex;
    flex-direction: column;
    height: 332px;
    width: 345px;
    align-items: baseline;
}

.excursion-image {
    height: 200px;
    width: 345px;

 
}

.rank-container{
    display: flex;
    flex-direction: row;
    gap: 3px;
}
.rank{
    font-size: 14px;
}
.inf{
    font-size: 12px;
    color: #999999;

}

.name{
    font-size: 16px;
    font-weight: 700;
    color: #23262F;

}

.price-container{
    display: flex;
    flex-direction: column;
    gap: 3px;
}
.price{
    font-size: 22px;
    font-weight: 700;
    color: #444444;

}


</style>
