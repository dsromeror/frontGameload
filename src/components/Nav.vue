<template>
  <div class="nav__menu">
    <a>¿Qienes Somos?</a>
    <a>Registrarse</a>
    <!-- <Button title="Inicio" class="regular" v-on:click="$router.push({name:'root'})" /> -->
    <Button title="Iniciar Sesión" class="regular" v-on:click="$router.push({name:'user_auth'})" />
    <!-- <Button title="Registrarse" class="regular" v-on:click="$router.push({name:'newuser'})" v-if="!is_auth" /> -->
    <Button title="Productos" class="regular" v-on:click="products"  />
    <!-- <Button title="Mi Carrito" class="regular" v-on:click="car"  />   -->
    <Button title="Ordenes" class="regular" v-on:click="order"  />
    <Button title="Cerrar Sesión" class="optional" v-on:click="logOut"  />
  </div>
</template>

<script>
import Button from './Button.vue'
import gql from 'graphql-tag'

export default {
  name: 'Nav',
  components: { Button },

  data: function(){
    return{
      is_auth: false
    }
},

created: function(){
  this.updateAccessToken();
},

methods:{

  updateAccessToken: async function(){
    if(localStorage.getItem('refresh_token')==null){
      this.$router.push({name: "user_auth"})
      this.is_auth = false
      return;
  }

  await this.$apollo
    .mutate({
      mutation: gql`
        mutation ($refreshTokenRefresh: String!) {
          refreshToken(refresh: $refreshTokenRefresh) {
            access
          }
        }
      `,
    variables: {
      refreshTokenRefresh: localStorage.getItem('refresh_token')
    }
  })
  .then((result) => {
    localStorage.setItem('access_token', result.data.refreshToken.access)
    this.is_auth = true
  })
  .catch((error) => {
      alert("Su sesión expiró, vuelva a iniciar sesión.")
      this.$router.push({name: "user_auth"})
      this.is_auth = false
      localStorage.clear();
    });
  },

  logIn: async function(data, username){
    localStorage.setItem('access_token', data.access)
    localStorage.setItem('refresh_token', data.refresh)
    localStorage.setItem('user_id', data.user_id)
    localStorage.setItem('current_username', username)
    
    await this.updateAccessToken();
    if(this.is_auth) this.init();
  },

  init: function(){
    this.$router.push({
      name: "user",
      params:{ username: localStorage.getItem("current_username") }
    })
  },

  account: function () {
    this.$router.push({
      name: "account",
      params: { username: localStorage.getItem("current_username") },
    });
  },

  products: function () {
    this.$router.push({
      name: "products",
      params: { username: localStorage.getItem("current_username") },
    });
  },

  newuser: function () {
    this.$router.push({
      name: "newuser"
    });
  },

  car: function () {
    this.$router.push({
      name: "car",
      params: { username: localStorage.getItem("current_username") },
    });
  },

  order: function () {
    this.$router.push({
      name: "order",
      params: { username: localStorage.getItem("current_username") },
    });
  },

  transacction: function () {
    this.$router.push({
      name: "transacction",
      params: { username: localStorage.getItem("current_username") },
    });
  },

  historial: function () {
    this.$router.push({
      name: "historial",
      params: { username: localStorage.getItem("current_username") },
    });
  },
  logOut: async function(){
    localStorage.removeItem('access_token')
    localStorage.removeItem('refresh_token')
    localStorage.removeItem('user_id')
    localStorage.removeItem('current_username')
      
    await this.updateAccessToken();
    },
  },
};
</script>

<style>
.nav__menu {
  align-items: center;
  margin-left: auto;
  margin-right: 20px;
  display: flex;
  height: 65px;
  padding: 0 50px;
}
.nav__menu a {
  color: var(--secundary-color);
  padding: 0 12px;
  display: flex;
  text-decoration: none;
  cursor: pointer;
  font-size: var(--size);
}
</style>