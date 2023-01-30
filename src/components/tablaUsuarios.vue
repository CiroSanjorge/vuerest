<template>
  <div id="tabla-usuarios">
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Usuario</th>
          <th scope="col">Nombre</th>
          <th scope="col">Email</th>
          <th scope="col">Ciudad</th>
          <th scope="col">Teléfono</th>
          <th scope="col" colspan="2">Operaciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="usuario in usuarios" :key="usuario.id">
          <td v-if="editando === usuario.id">
            <label>{{ usuario.id }}</label>
          </td>
          <td v-else>{{ usuario.id }}</td>

          <td v-if="editando === usuario.id">
            <input
              type="text"
              class="form-control"
              v-model="usuario.username"
            />
          </td>
          <td v-else>{{ usuario.username }}</td>

          <td v-if="editando === usuario.id">
            <input type="text" class="form-control" v-model="usuario.name" />
          </td>
          <td v-else>{{ usuario.name }}</td>

          <td v-if="editando === usuario.id">
            <input type="text" class="form-control" v-model="usuario.email" />
          </td>
          <td v-else>{{ usuario.email }}</td>

          <td v-if="editando === usuario.id">
            <input
              type="text"
              class="form-control"
              v-model="usuario.city"
            />
          </td>
          <td v-else>{{ usuario.city }}</td>
          <td v-if="editando === usuario.id">
            <input type="text" class="form-control" v-model="usuario.phone" />
          </td>

          <td v-else>{{ usuario.phone }}</td>
          <td v-if="editando === usuario.id">
            <button
              type="button"
              class="btn btn-sm btn-success"
              @click="actualizarUsuario(usuario)"
            >
              💾 Guardar
            </button>
            <button
              type="button"
              class="btn btn-sm btn-secondary"
              @click="cancelarEdicion(usuario)"
            >
              ❌ Cancelar
            </button>
          </td>
          <td v-else>
            <button
              type="button"
              class="btn btn-sm btn-info"
              @click="editarUsuario(usuario)"
            >
              ✏Editar
            </button>
            <button
              type="button"
              class="btn btn-sm btn-danger"
              @click="$emit('eliminar-usuario', usuario)"
            >
              🗑 Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "tablaUsuarios",
  props: {
    usuarios: Array,
  },
  data() {
    return {
      editando: null,
    };
  },
  methods: {
    editarUsuario(usuario) {
      this.usuarioEditado = Object.assign({}, usuario);
      this.editando = usuario.id;
    },
    cancelarEdicion(usuario) {
      Object.assign(usuario, this.usuarioEditado);
      this.editando = null;
    },
    actualizarUsuario(usuario) {
      if (
        !usuario.name.length ||
        !usuario.username.length ||
        !usuario.email.length ||
        !usuario.city.length ||
        !usuario.phone.length
      ) {
        return;
      }
      this.$emit("actualizarUsuario", usuario);
      this.editando = null;
    },
  },
};
</script>

<style>
</style>