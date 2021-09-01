<template>
  <div class="wraper__subNav">
    <a v-on:click="$router.push({name:'root'})">Inicio</a>
    <a href="#">Playstation</a>
    <a href="#">Xbox</a>
    <a href="#">Nintendo</a>
    <a href="#">Acesorios</a> 
    <a class="carrito" v-on:click="car">$0 <i class="fas fa-shopping-cart"></i></a>
  </div>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'SubNav',
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

  
  car: function () {
    this.$router.push({
      name: "car",
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
.wraper__subNav {
  width: 100%;
  background:var(--medium-blue);
  height: 45px;
  padding: 0 10%;
  display: flex;
}
.wraper__subNav a {
  color: var(--secundary-color);
  margin: auto;
  align-items: center;
  font-weight: 700;
  text-decoration: none;
  cursor: pointer;
}
.carrito {
  background: var(--heavy-blue);
  padding: 5px 10px;
  border-radius: 20px;
}
.carrito:hover {
  background: #494949;
}
</style>