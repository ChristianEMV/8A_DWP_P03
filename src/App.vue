<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h3>Formulario de Registro</h3>
      </div>
      <div class="card-body">

    git<!--Body Formulario -->

      <!--Area nombre-->
    <form @submit="submitForm" v-if="showForm">
      
      <b-form-group label="Nombre(s)" label-for="nombre">
        <b-form-input
          id="nombre"
          v-model="formulario.nombre"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Apellido paterno" label-for="apellidoPaterno">
        <b-form-input
          id="apellidoPaterno"
          v-model="formulario.apellidoPaterno"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Apellido materno" label-for="apellidoMaterno">
        <b-form-input
          id="apellidoMaterno"
          v-model="formulario.apellidoMaterno"
        ></b-form-input>
      </b-form-group>

      <!--Area dirección-->
      <b-form-group label="Código postal" label-for="codigoPostal">
        <b-form-input
          id="codigoPostal"
          v-model="formulario.codigoPostal"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Calle" label-for="calle">
        <b-form-input
          id="calle"
          v-model="formulario.calle"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Número" label-for="numero">
        <b-form-input
          id="numero"
          v-model="formulario.numero"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Ciudad" label-for="ciudad">
        <b-form-input
          id="ciudad"
          v-model="formulario.ciudad"
          required
        ></b-form-input>
      </b-form-group>

      <!--Area fecha de nacimiento-->
      <b-form-group label="Fecha de nacimiento" label-for="fechaNacimiento">
        <b-form-input
          type="date"
          id="fechaNacimiento"
          v-model="formulario.fechaNacimiento"
          :max="maxFechaNacimiento"
          required
        ></b-form-input>
      </b-form-group>

      
      <!--Area correo electronico-->
      <b-form-group label="Correo electrónico" label-for="email">
        <b-form-input
          type="email"
          id="email"
          v-model="formulario.email"
          required
        ></b-form-input>
      </b-form-group>

      <!--Area número telefonico-->
      <b-form-group label="Número telefónico" label-for="telefono">
        <b-form-input
          id="telefono"
          v-model="formulario.telefono"
          required
        ></b-form-input>
      </b-form-group>
      
      <!--Area fotografía-->
      <b-form-group label="Fotografía" label-for="foto">
        <b-form-file id="foto" v-model="formulario.foto" required></b-form-file>
      </b-form-group>

      <!-- Boton enviar --> 
      <b-button type="submit" variant="primary">Enviar</b-button>
    </form>
    <div v-else>
      <h2>¡Formulario enviado!</h2>
      <p>Nombre completo: {{ formulario.nombreCompleto }}</p>
      <p>Dirección: {{ formulario.direccion }}</p>
      <p>Fecha de nacimiento: {{ formulario.fechaNacimiento }}</p>
      <p>Correo electrónico: {{ formulario.email }}</p>
      <p>Número telefónico: {{ formulario.telefono }}</p>
    </div>

      </div>

    </div>

  </div>
</template>

<script>
import {
  BForm,
  BFormGroup,
  BFormInput,
  BFormFile,
  BButton,
} from "bootstrap-vue";

export default {
  components: {
    BForm,
    BFormGroup,
    BFormInput,
    BFormFile,
    BButton,
  },
  data() {
    return {
      formulario: {
        nombre: "",
        apellidoPaterno: "",
        apellidoMaterno: "",
        codigoPostal: "",
        calle: "",
        numero: "",
        ciudad: "",
        fechaNacimiento: "",
        email: "",
        telefono: "",
        foto: null,
      },
      showForm: true,
    };
  },
  computed: {
    nombreCompleto() {
      return `${this.formulario.nombre} ${this.formulario.apellidoPaterno} ${this.formulario.apellidoMaterno}`.trim();
    },
    direccion() {
      return `${this.formulario.calle} ${this.formulario.numero}, ${this.formulario.ciudad}, ${this.formulario.codigoPostal}`;
    },
    maxFechaNacimiento() {
      const currentDate = new Date();
      const maxDate = new Date(
        currentDate.getFullYear() - 18,
        currentDate.getMonth(),
        currentDate.getDate()
      );
      return maxDate.toISOString().split("T")[0];
    },
  },
  methods: {
    submitForm() {
      if (this.validarFormulario()) {
        // Validar correo electrónico
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(this.formulario.email)) {
          alert("Por favor, ingresa un correo electrónico válido.");
          return;
        }

        // Validar número telefónico
        if (this.formulario.telefono.length !== 10) {
          alert("El número telefónico debe tener exactamente 10 dígitos.");
          return;
        }

        // Validar tamaño de la fotografía
        const maxSizeInBytes = 3 * 1024 * 1024; // 3 MB
        if (
          this.formulario.foto &&
          this.formulario.foto.size > maxSizeInBytes
        ) {
          alert("El tamaño de la fotografía debe ser menor a 3 MB.");
          return;
        }

        //Validar fecha de nacimiento
        const fechaNacimiento = new Date(this.formulario.fechaNacimiento);
        const edad = Math.floor(
          (Date.now() - fechaNacimiento.getTime()) / (1000 * 60 * 60 * 24 * 365)
        );
        if (edad < 18) {
          alert("Debes tener al menos 18 años para enviar el formulario.");
          return;
        }

        console.log(this.formulario);
        this.showForm = false;
      }
    },
    validarFormulario() {
      if (
        !this.formulario.nombre ||
        !this.formulario.apellidoPaterno ||
        !this.formulario.codigoPostal ||
        !this.formulario.calle ||
        !this.formulario.numero ||
        !this.formulario.ciudad ||
        !this.formulario.fechaNacimiento ||
        !this.formulario.email ||
        !this.formulario.telefono
      ) {
        alert("Por favor, completa todos los campos que sean obligatorios.");
        return false;
      }

      return true;
    },
  },
};
</script>
<style scoped>
.container {
    max-width: 500px;
    margin: 0 auto;
    padding: 10px;
  }
  
  .card {
    border: 1px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 10px 10px rgba(0, 0, 0, 0.1);
  }
  
  .card-header {
    color:#ffff;
    padding: 10px;
    background-color: #2a63ff;
    border-bottom: 1px solid #ccc;
  }
  
  .card-body {
    padding: 20px;
  }
</style>