<template>
  
  <div class="wrapper__login">
      <h2 class="nameCataloge">Lista de juegos</h2>
      <section class="products">
      <Cards v-for="product in products" 
      :key="product.title" 
      :product="product"/> 
      </section>
  </div>
</template>

<script>
import Cards from './Cards.vue'

export default {
  components: { Cards },
  data(){
    return{
    products: [
        {
          title: 'Batman',
          price: '$20.000 COP',
          category: 'Juegos',
      //     src: '../assets/logo.png'
        },
        {
          title: 'Fifa21',
          price: '$20.000 COP',
          category: 'Juegos',
      //     src: require('./assets/products/fifa21.jpeg')
        },
        {
          title: 'Marvel spider man',
          price: '$20.000 COP',
          category: 'Juegos',
      //     src: require('./assets/products/marvel-s-spider-man.jpg')
        },
        {
          title: 'GTA 5',
          price: '$20.000 COP',
          category: 'Juegos',
      //     src: require('./assets/products/marvel-s-spider-man.jpg')
        }
      ]
    }
  }
}
</script>

<style>
.main-wraper {
  max-width: 1000px;
  align-content: center;
}
.nameCataloge {
      font-family: var(--ff-secondary);
      font-weight: 700;
      text-transform: uppercase;
      padding-left: 32px;
      font-size: 2em;
}
.products {
      display: inline-flex;
}
</style>