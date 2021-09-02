<template>
  <div id="app" class="app">
    <div class="header"> 

      <Header />
      <SubNav />
      
    </div>.

    <div class="main-component">
      <router-view v-on:log-in="logIn"></router-view>
    </div>

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
      productosDisponibles: [],
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