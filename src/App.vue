<template>
  <div id="app">
    <nav>
      <form class="form">
        <input
          v-model="nombre"
          @keyup.enter="crearPaciente"
          type="text"
          class="form-control"
          placeholder="Nombre"
        />
        <input
          v-model="edad"
          @keyup.enter="crearPaciente"
          type="number"
          class="form-control"
          placeholder="Edad"
        />
      </form>
      <button @click="crearPaciente" class="btn btn-success">Añadir</button>
    </nav>
    <section>
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">Id</th>
            <th scope="col">Nombre</th>
            <th scopte="col">Edad</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(paciente, index) in pacientes">
            <td>{{ paciente.id }}</td>
            <td>
              <span v-if="isActive && idActualizar == index">
                <!-- Formulario para actualizar -->
                <input
                  v-model="nombreActualizar"
                  type="text"
                  class="form-control"
                />
              </span>
              <span v-else>
                {{ paciente.nombre }}
              </span>
            </td>
            <td>
              <span v-if="isActive && idActualizar == index">
                <!-- Formulario para actualizar -->
                <input
                  v-model="edadActualizar"
                  type="text"
                  class="form-control"
                />
              </span>
              <span v-else>
                {{ paciente.edad }}
              </span>
            </td>
            <td>
              <span v-if="isActive && idActualizar == index">
                <button
                  @click="guardarPacientes(index)"
                  class="btn btn-success"
                >
                  Guardar
                </button>
              </span>
              <span v-else>
                <!-- Botón para mostrar el formulario de actualizar -->
                <button
                  @click="verFormActualizar(index)"
                  class="btn btn-warning"
                >
                  Actualizar
                </button>
                <!-- Botón para borrar -->
                <button @click="borrarPaciente(index)" class="btn btn-danger">
                  Borrar
                </button>
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      nombre: "",
      edad: "",
      isActive: false,
      idActualizar: -1,
      nombreActualizar: "",
      edadActualizar: "",
      pacientes: [],
    };
  },
  methods: {
    crearPaciente: function () {
      //añadiendo pacientes
      this.pacientes.push({
        id: +new Date(),
        nombre: this.nombre,
        edad: this.edad,
      });
      //vaciamos el formulario añadir
      this.nombre = "";
      this.edad = "";
    },
    verFormActualizar: function (paciente_id) {
      // Antes de mostrar el formulario de actualizar, rellenamos sus campos
      this.idActualizar = paciente_id;
      this.nombreActualizar = this.pacientes[paciente_id].nombre;
      this.edadActualizar = this.pacientes[paciente_id].edad;
      // Mostramos el formulario
      this.isActive = true;
    },
    borrarPaciente: function (paciente_id) {
      // Borramos de la lista
      this.pacientes.splice(paciente_id, 1);
    },
    guardarPacientes: function (paciente_id) {
      this.isActive = false;
      this.pacientes[paciente_id].nombre = this.nombreActualizar;
      this.pacientes[paciente_id].edad = this.edadActualizar;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
