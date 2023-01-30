<template>
  <div class="container" id="id">
    <div class="row">
      <div class="col-md-12 mt-2">
        <h3>GESTIÓN CLIENTES</h3>
        <div class="row">
          <div class="col-md-12">
            <formulario-usuarios @alta-usuario="postUsuario" ></formulario-usuarios>
            <tabla-usuarios :usuarios="usuarios" @eliminar-usuario="deleteUsuario" @editar-usuario="editarUsuario"
              @actualizar-usuario="putUsuario"></tabla-usuarios>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TablaUsuarios from "./components/tablaUsuarios.vue";
import FormularioUsuarios from "./components/formularioUsuarios.vue";
export default {
  name: "App",
  components: {
    TablaUsuarios,
    FormularioUsuarios
  },
  data() {
    return {
      usuarios: [],
    };
  },
  methods: {
    async postUsuario(usuario) {
      try {

        const response = await fetch("http://localhost:3000/usuarios", {
          method: "POST",
          body: JSON.stringify(usuario),
          headers: { 'Content-Type': 'application/json; charset=UTF-8' },
        });

        const usuarioCreado = await response.json();
        this.usuarios = [...this.usuarios, usuarioCreado];
      } catch (error) {
        console.log(error);
      }
    },
    async getUsuarios() {
      try {

        const response = await fetch("http://localhost:3000/usuarios");
        this.usuarios = await response.json();
      } catch (error) {
        console.log("Error en getUsuarios", error);
      }
    },
    async deleteUsuario(usuario) {
      try {
        const response = await fetch(
          `http://localhost:3000/usuarios/${usuario.id}`,
          {
            method: "DELETE",
          }
        );
        //this.getUsuarios();
        this.usuarios = this.usuarios.filter((u) => u.id !== usuario.id);
      } catch (error) {
        console.log("Error en deleteUsuario", error);
      }
    },
    async putUsuario(usuario) {
      try {
        const response = await fetch(
          `http://localhost:3000/usuarios/${usuario.id}`,
          {
            method: "PUT",
            body: JSON.stringify(usuario),
            headers: { "Content-Type": "application/json; charset=UTF-8" },
          }
        );
        const usuarioActualizado = await response.json();
        this.usuarios = this.usuarios.map((u) =>
          u.id === usuario.id ? usuarioActualizado : u
        );
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {

    this.getUsuarios();
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
