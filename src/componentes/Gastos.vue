<template>
  <section class="src-componentes-ingreso">
    <div class="jumbotron">
      <h2>Formulario de gastos</h2>
      <hr />

      <vue-form :state="formstate" @submit.prevent="enviar()">
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            v-model="formData.nombre"
            required
            name="nombre"
            autocomplete="off"
            class="form-control"
          />

          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>
        <br />

        <validate tag="div">
          <label for="descripcion">Descripcion</label>
          <input
            type="text"
            id="descripcion"
            v-model="formData.descripcion"
            required
            name="descripcion"
            autocomplete="off"
            class="form-control"
          />

          <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>
        <br />

        <validate tag="div">
          <label for="importe">Importe</label>
          <input
            type="number"
            id="importe"
            v-model.number="formData.importe"
            required
            name="importe"
            autocomplete="off"
            class="form-control"
          />

          <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
          </field-messages>
        </validate>

        <br />

        <button
          class="btn btn-success my-3"
          :disabled="formstate.$invalid"
          type="submit"
        >
          subir gastos
        </button>
      </vue-form>
      <button class="btn btn-success my-3" @click="getgastos()">
        Cargar planilla con gastos
      </button>
      <br />
      <hr />

      <h2>Resumen de gastos</h2>
      <br />

      <div v-if="gastos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Importe</th>
            <th>Fecha</th>
          </tr>
          <tr v-for="(gasto, index) in gastos" :key="index">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.descripcion }}</td>
            <td>${{ gasto.importe }}</td>
            <td>{{ gasto.fecha }}</td>
          </tr>
          <tr>
            <td>TOTAL GASTO</td>
            <td>{{ "$" + getGastoTotal() }}</td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info" style="color: red">
        No hay gastos ingresados
      </h3>
    </div>

    <div class="jumbotron">
      <table>
        <tr>
          <th>Preguntas</th>
        </tr>

        <tr>
          <td>Pregunta 1: Como es la forma reducida de v-on:click?</td>

          <td>Respuesta: @click</td>
        </tr>

        <tr>
          <td>Pregunta 2: vue create se utiliza para</td>

          <td>Respuesta: Crear un proyecto base Vue basado en Vue CL</td>
        </tr>

        <tr>
          <td>Pregunta 3: El modificador trim recorta..</td>

          <td>
            Respuesta: Saca los espacios intermedios puestos en una entrada de
            texto
          </td>
        </tr>

        <tr>
          <td>
            Pregunta 4: ¿La propiedad routes en la definición del router de
            Vue.js tiene como función:
          </td>

          <td>
            Respuesta: Definir el path y el componente activo para esa ruta
          </td>
        </tr>

        <tr>
          <td>Pregunta 5: Módulos de Vuex</td>

          <td>
            Respuesta: La lógica para cambiar el estado global de la aplicación
          </td>
        </tr>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  name: "src-componentes-ingreso",
  props: [],
  mounted() {},
  data() {
    return {
      formstate: {},
      formData: this.getInitialData(),
      urt: "https://63800bab8efcfcedacfbd401.mockapi.io/Gastos",
      gastos: [],
      acumulador: 0,
    };
  },
  methods: {
    getInitialData() {
      return {
        nombre: null,
        descripcion: null,
        importe: null,
      };
    },

    async getgastos() {
      try {
        let response = await this.axios(this.url);
        let respuesta = response.data;
        //console.log(respuesta)
        this.gastos = respuesta;
      } catch (error) {
        console.error("Aca esta el error:", error);
      }
    },

    getGastoTotal() {
      return this.acumulador;
    },

    enviar() {
      let gasto = { ...this.formData };
      gasto.fecha = new Date().toLocaleString();

      console.log(gasto);
      this.gastos.push(gasto);
      this.acumulador += gasto.importe;

      this.formData = this.getInitialData();
      this.formstate._reset();
    },
  },
  computed: {},
};
</script>

<style scoped lang="css">
.src-componentes-ingreso {
}

.jumbotron {
  background-color: rgb(155, 181, 244);
  color: brown;
}

hr {
  background-color: rgb(74, 16, 16);
}

h2 {
  color: rgb(74, 16, 16);
}

label {
  color: rgb(74, 16, 16);
}

pre {
  color: rgb(231, 246, 102);
}
</style>
