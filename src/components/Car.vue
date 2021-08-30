<template>
  <div id="Carrito">
    <table>
      <tr>
        <th>Producto Id</th>
        <th>Cantidad</th>
        <th>Precio</th>
        <th>Disminuir</th>
      </tr>

      <tr v-for="product in carritoByUsuarioId" :key="product.producto_id">
        <td>{{ product.producto_id}}</td>
        <td>{{ product.cantidad}}</td>
        <td>${{ product.precio }} COP</td>
        <td><button v-on:click="add">Disminuir</button></td>
      </tr>
    </table>
  </div>
</template>


<script>
import gql from "graphql-tag";

export default {
  name: "Car",

  data: function () {
    return {
      usuarioId: localStorage.getItem("user_id"),
      carritoByUsuarioId: [],
    };
  },

  created: function () {
    this.username = this.$route.params.username;
  },

  apollo: {
    carritoByUsuarioId: {
      query: gql`
        query Query($carritoByUsuarioIdUsuarioId: String!) {
          carritoByUsuarioId(usuarioId: $carritoByUsuarioIdUsuarioId) {
            carrito_id
            usuarioId
            productoId
            productoCantidad
            productoPrecio
          }
        }
      `,
      variables() {
        return {
          carritoByUsuarioId: localStorage.getItem("user_id"),
        };
      },
    },
  },
};

</script>

<style>
#Carrito {
  width: 100%;
  height: 120%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

#Carrito table {
  width: 50%;
  border-collapse: collapse;
  border: 1px solid rgba(0, 0, 0, 0.3);
  border-radius: 20px;
}

#Carrito table td,
#Carrito table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#Carrito table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#Carrito table tr:hover {
  background-color: #ddd;
}

#Carrito table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: crimson;
  color: white;
}
</style>