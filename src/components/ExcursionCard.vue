<template>
  <div class="excursion-card">
    <img :src="excursion.main_photo.original" alt="img" class="excursion-image" />
    <div class="rank-container">
        <img src="./rate.png" alt="img" class="rate-image" />
        <p class="rank"> {{ excursion.customers_review_rating }}</p>
        <p class="inf"> ({{ excursion.reviews }})</p>
    </div>
    <div class="name">{{ excursion.title }}</div>
    <div class="price-container">
        <div class="price"> от {{ excursion.netto_price }}</div>
        <div class="inf"> за экскурсию</div>
    </div>
   
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
    display: flex;
    flex-direction: column;
    height: 332px;
    width: 345px;
    align-items: baseline;
}

.excursion-image {
    height: 200px;
    width: 345px;
    object-fit: cover;
 
}

.rank-container{
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 3px;
}
.rank{
    font-size: 14px;
}

.rate-image{
    height: 15px;
    widows: 15px;
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
    gap: 0px;
}
.price{
    font-size: 22px;
    font-weight: 700;
    color: #444444;
    margin-top:10px;

}


</style>
