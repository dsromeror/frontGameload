<template>
  <div id="Productos">
    <table>
      <tr>
        <th>Producto Id</th>
        <th>Categoria</th>
        <th>Codigo</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Imagen</th>
        <th>Agregar</th>
      </tr>

      <tr v-for="product in productosDisponibles" :key="product.producto_id">
        <td>{{ product.producto_id}} </td>
        <td>{{ product.categoria}}</td>
        <td>{{ product.codigo }}</td>
        <td>{{ product.nombre }}</td>
        <td>${{ product.precio }} COP</td>
        <td>${{ product.imagen }} COP</td>
        <td><button v-on:click="add(product.producto_id)">Agregar</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "Products",

  data: function () {
    return {
      productosDisponibles: [],
      agregarCarritoByUsuarioIdProducto: {
        producto_id: "",
      },
    };
  },

  created: function () {
    this.username = this.$route.params.username;
  },

  apollo: {
    productosDisponibles: {
      query: gql`
        query Query {
          productosDisponibles {
            producto_id
            categoria
            codigo
            nombre
            precio
            imagen
          }
        }
      `,
      variables() {
        return {
          productosDisponibles: localStorage.getItem("user_id"),
        };
      },
    },
  },

  methods:{
    add: async function(producto_id){
    this.agregarCarritoByUsuarioIdProducto.producto_id = producto_id
    await this.$apollo
      .mutate({
        mutation: gql`
          mutation ($agregarCarritoByUsuarioIdUsuarioId: String!, $agregarCarritoByUsuarioIdProducto: ProductoInput) {
            agregarCarritoByUsuarioId(usuarioId: $agregarCarritoByUsuarioIdUsuarioId, producto: $agregarCarritoByUsuarioIdProducto) {
              carrito_id
              usuarioId
              productoId
              productoNombre
              productoCantidad
              productoPrecio
            }
          }
        `,  
        variables:{
          agregarCarritoByUsuarioIdUsuarioId: localStorage.getItem("user_id").replace(/^(0+)/g, ''),
          agregarCarritoByUsuarioIdProducto: this.agregarCarritoByUsuarioIdProducto,
        },
      })
      .then((result) => {
        alert("Producto agregado de forma correcta")
      })
      .catch((error) => {
          alert("Error");
      });
    },
  }
  
};

</script>

<style>
#Productos {
  width: 100%;
  height: 120%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

#Productos table {
  width: 50%;
  border-collapse: collapse;
  border: 1px solid rgba(0, 0, 0, 0.3);
  border-radius: 20px;
}

#Productos table td,
#Productos table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#Productos table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#Productos table tr:hover {
  background-color: #ddd;
}

#Productos table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: var(--medium-blue);
  color: white;
}
</style>