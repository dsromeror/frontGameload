<template>
  <div id="Carrito">
    <table>
      <tr>
        <th>Producto Id</th>
        <th>Nombre</th>
        <th>Cantidad</th>
        <th>Precio</th>
        <th>Disminuir</th>
      </tr>

      <tr v-for="product in carritoByUsuarioId" :key="product.producto_id">
        <td>{{ product.productoId}}</td>
        <td>{{ product.productoNombre}}</td>
        <td>{{ product.productoCantidad}}</td>
        <td>${{ product.productoPrecio }} COP</td>
        <td><button v-on:click="remove(product.producto_id)">Disminuir</button></td>
      </tr>
    </table>
    <h2>Total: </h2>
    <button v-on:click="confirm">Confirmar carrito</button>
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
      crearOrdenUsuarioId: "null",
      disminuirProductoByCarrito: {
        producto_id: "",
      },
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
            productoNombre
            productoCantidad
            productoPrecio
          }
        }
      `,
      variables() {
        return {
          carritoByUsuarioIdUsuarioId: localStorage.getItem("user_id").replace(/^(0+)/g, ''),
        };
      },
    },
  },

  methods:{
    confirm: async function(){
    await this.$apollo
      .mutate({
        mutation: gql`
          mutation CrearOrdenMutation($crearOrdenUsuarioId: String!) {
            crearOrden(usuarioId: $crearOrdenUsuarioId) {
              fechaCompra
              codigoOrden
              usuarioId
              productoCantidad
              productoPrecio
              estado
            }
          }
        `,  
        variables:{
          crearOrdenUsuarioId: localStorage.getItem("user_id"),
        },
      })
      .then((result) => {
        alert("Orden creada")
      })
      .catch((error) => {
          alert("Error");
      });
    },

    remove: async function(producto_id){
    this.CarritoInput.producto_id = producto_id
    await this.$apollo
      .mutate({
        mutation: gql`
          mutation ($disminuirProductoByCarritoCarrito: CarritoInput!) {
            disminuirProductoByCarrito(carrito: $disminuirProductoByCarritoCarrito) {
              carrito_id
              usuarioId
              productoId
              productoCantidad
              productoPrecio
            }
          }
        `,  
        variables:{
          disminuirProductoByCarritoCarrito: localStorage.getItem("user_id").replace(/^(0+)/g, ''),
          //CarritoInput: this.CarritoInput,
        },
      })
      .then((result) => {
        alert("Reducción de producto exitoso")
      })
      .catch((error) => {
          alert("Error");
      });
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