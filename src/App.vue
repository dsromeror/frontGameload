<template>
  <div id="app" class="app">
    <div class="header">
  
      <nav>
        <button v-on:click="$router.push({name:'root'})" v-if="!is_auth"> Inicio </button>
        <button v-on:click="$router.push({name:'user_auth'})" v-if="!is_auth"> Iniciar Sesión </button>
        <button v-on:click="$router.push({name:'newuser'})" v-if="!is_auth"> Registrarse </button>
        <button v-on:click="init" v-if="is_auth" > Inicio </button>
        <button v-on:click="products" v-if="is_auth" > Productos </button><!--cambiar luego, debe renderizar los productos disponibles-->
        <button v-on:click="car" v-if="is_auth" > Mi Carrito </button><!--Cambiar y renderizar los productos cargados-->
        <button v-on:click="order" v-if="is_auth" > Ordenes </button><!--Cambiar a la orden confirmada y precio total-->
        <button v-on:click="logOut" v-if="is_auth" > Cerrar Sesión </button>
      </nav>

      <Header />
      <SubNav />
    </div>.

    <div class="main-component">
      <router-view v-on:log-in="logIn"></router-view>
    </div>

    <!-- <div class="all-footer">

      <figure class="logo-footer">
          <img src="./assets/Logos/logoWhite.png" alt="Logo">
      </figure>
    
      <div class="info">
        <a href="#">Tienda</a>
        <a href="#">Mas reciente</a>
        <a href="#">Accesorios premium</a>
        <a href="#">Suscripción</a>
      </div>

      <div class="servicio">
        <a href="#">Servicio al cliente</a>
        <a href="#">pedidos@gamesload.com</a>
        <a href="#">Cll 78 # 40-5 Bogotá, Colombia</a>
        <a href="#">(+57) 369 874 21 45  -  478 214 21</a>
      </div>

      <div class="socilamedia">
        <h2>Follow us</h2>
        <i id="facebook" class="fab fa-facebook"></i>
        <i id="youtube" class="fab fa-youtube"></i>
        <i id="insta" class="fab fa-instagram"></i>
      </div>
    </div> -->
    <div class="footer">
      <Footer />
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import SubNav from './components/SubNav.vue'
import Footer from './components/Footer.vue'
import gql from 'graphql-tag'

export default {
  name: 'App',
  components: { Header, SubNav, Footer },
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
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@200;300;400;500;600;700&family=Roboto:wght@100;300;400;500;900&display=swap');

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
:root {
  --primary-color: #0D0D0D;
  --palette-pink: #EE05F2 ;
  --heavy-blue: #0378A6;
  --medium-blue: #04ADBF;
  --light-blue: #04D9C4;
  --secundary-color: #fff;
  --size: 18px;
  --ff-primary: 'Roboto', sans-serif;
  --ff-secondary: 'Oswald', sans-serif;
  --transition: all 0.3s linear;
  --spacing: 0.1rem;
  --radius: 0.25rem;
  --light-shadow: 0 5px 15px rgba();
  --dark-shadow: 0 5px 15px rgba();
  --max-width: 1170px;
  --fixed-width: 620px;
  }
*{
  box-sizing: border-box;
}
footer {
  bottom: 0;
  left: 0px;
  width: 100%;
}

</style>