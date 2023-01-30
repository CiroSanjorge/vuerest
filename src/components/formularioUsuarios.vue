<template>
  <div id="formulario-usuario">
    <form @submit.prevent="enviarForm">
      <div class="container">
        <div class="row">
          <div class="col-md-4">
            <div class="form-group">
              <label>Nombre</label>
              <input ref="name" v-model='usuario.name' type="text" class="form-control"
                :class="{ 'is-invalid': procesando && nameInvalido }" @focus='resetEstado' @keypress='resetEstado' />
            </div>
          </div>
          <div class="col-md-4">
            <div class="form-group">
              <label>Email</label>
              <input v-model='usuario.email' type="email" class="form-control"
                :class="{ 'is-invalid': procesando && emailInvalido }" @focus='resetEstado' @keypress='resetEstado' />
            </div>
          </div>
          <div class="col-md-4">
            <div class="form-group">
              <label>Teléfono</label>
              <input v-model='usuario.phone' type="text" class="form-control"
                :class="{ 'is-invalid': procesando && phoneInvalido }" @focus='resetEstado' @keypress='resetEstado' />
            </div>
          </div>
        </div>
        <div class='row'>
          <div class='col-md-12'>
            <div class='form-group'>
              <button class="btn btn-primary">Alta Usuario</button>
            </div>
          </div>
        </div>
      </div>
      <div class='container'>
        <div class='row'>
          <div class='col-md-12'>
            <div v-if="error && procesando" class="alert alert-danger" role="alert">
              Debes cubrir todos los campos
            </div>
            <div v-if='correct' class='alert alert-success' role="alert">
              Usuario dado de alta correctamente
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "formularioUsuarios",
  data() {
    return {
      procesando: false,
      correcto: false,
      error: false,
      usuario: {
        id: "",
        username: "",
        name: "",
        email: "",
        city: "",
        phone: "",
      }
    }
  },
  methods: {
    enviarForm() {
      this.procesando = true;
      this.resetEstado();
      if (this.nameInvalido || this.emailInvalido || this.phoneInvalido) {
        this.error = true;
        return;
      }
      this.$emit('alta-usuario', this.usuario);
      this.$refs.name.focus();
      this.error = false;
      this.correcto = true;
      this.procesando = false;

      this.usuario = {
        id: "",
        username: "",
        name: "",
        email: "",
        city: "",
        phone: "",
      }
    },
    resetEstado() {
      this.correcto = false;
      this.error = false;
    }
  },
  computed: {
    nameInvalido() {
      return this.usuario.name.length < 1;
    },
    emailInvalido() {
      return this.usuario.email.length < 1;
    },
    phoneInvalido() {
      return this.usuario.phone.length < 1;
    }
  }
}
</script>

<style scoped>

</style>