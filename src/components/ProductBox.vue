<template>
  <!-- <div class="" v-for="product in products" :key="product.id"> -->
    <div class="">
      <figure>
        <img :src="product.get_thumbnail">
      </figure>

      <h3>{{ product.name }}</h3>
      <p class="">${{ product.price }}</p>

      <router-link :to="product.get_absolute_url">
        View details 
      </router-link>
    </div>
  <!-- </div> -->
</template>

<script> 
export default {
  name: 'ProductBox',
  props: {
    product: Object
  }
}
</script>
