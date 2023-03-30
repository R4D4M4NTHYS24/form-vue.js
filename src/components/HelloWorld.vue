<template>
  <div class="container">
    <div>
      <h2>Paso 1</h2>
      <label for="pais">País</label>
      <select id="pais" v-model="paisSeleccionado">
        <option v-for="pais in paises" :value="pais" :key="pais">
          {{ pais }}
        </option>
      </select>

      <label for="genero">Género</label>
      <select id="genero" v-model="genero">
        <option v-for="genero in generos" :value="genero" :key="genero">
          {{ genero }}
        </option>
      </select>

      <label for="primer-nombre">Primer Nombre</label>
      <input type="text" id="primer-nombre" v-model="primerNombre" />

      <label for="segundo-nombre">Segundo Nombre</label>
      <input type="text" id="segundo-nombre" v-model="segundoNombre" />

      <label for="fecha-nacimiento">Fecha de Nacimiento</label>
      <input type="date" id="fecha-nacimiento" v-model="fechaNacimiento" />

      <label for="tipo-documento">Tipo de Documento</label>
      <select id="tipo-documento" v-model="tipoDocumento">
        <option value="cedula">Cedula de ciudadanía</option>
        <option value="pasaporte">Pasaporte</option>
        <option value="cedula-extranjeria">Cedula de extranjería</option>
      </select>

      <label for="numero-documento">Número de Documento</label>
      <input
        type="number"
        id="numero-documento"
        v-model="numeroDocumento"
        min="0"
      />

      <label for="foto-frente">Foto del Documento - Frente</label>
      <input
        type="file"
        id="foto-frente"
        accept=".jpg"
        @change="cargarFotoFrente"
      />

      <label for="foto-reverso">Foto del Documento - Reverso</label>
      <input
        type="file"
        id="foto-reverso"
        accept=".jpg"
        @change="cargarFotoReverso"
      />

      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        @click="avanzarPaso"
      >
        Siguiente
      </button>
    </div>
    <h2>Paso 2</h2>
    <label for="correo-electronico">Correo electrónico</label>
    <input
      type="email"
      class="form-control"
      id="correo-electronico"
      v-model="correoElectronico"
      @input="validarCampo('correoElectronico', $event.target.value)"
    />
    <i
      v-if="errores.includes('correoElectronico')"
      class="fa fa-times text-danger"
    ></i>
    <i v-else class="fa fa-check text-success"></i>

    <label for="contrasena">Contraseña</label>
    <input
      type="password"
      class="form-control"
      id="contrasena"
      v-model="contrasena"
      @input="validarCampo('contrasena', $event.target.value)"
    />
    <i
      v-if="errores.includes('contrasena')"
      class="fa fa-times text-danger"
    ></i>
    <i v-else class="fa fa-check text-success"></i>

    <label for="confirmar-contrasena">Confirmar contraseña</label>
    <input
      type="password"
      class="form-control"
      id="confirmar-contrasena"
      v-model="confirmarContrasena"
      @input="validarCampo('confirmarContrasena', $event.target.value)"
    />
    <i
      v-if="errores.includes('confirmarContrasena')"
      class="fa fa-times text-danger"
    ></i>
    <i v-else class="fa fa-check text-success"></i>

    <label for="telefono">Teléfono</label>
    <input
      type="tel"
      class="form-control"
      id="telefono"
      v-model="telefono"
      @input="validarCampo('telefono', $event.target.value)"
    />
    <i v-if="errores.includes('telefono')" class="fa fa-times text-danger"></i>
    <i v-else class="fa fa-check text-success"></i>

    <label for="celular">Celular</label>
    <input
      type="tel"
      class="form-control"
      id="celular"
      v-model="celular"
      @input="validarCampo('celular', $event.target.value)"
    />
    <i v-if="errores.includes('celular')" class="fa fa-times text-danger"></i>
    <i v-else class="fa fa-check text-success"></i>

    <div v-if="errores.length">
      <h3>Errores de Validación</h3>
      <ul>
        <li v-for="error in errores" :key="error">{{ error }}</li>
      </ul>
    </div>

    <button @click="avanzarPaso">Siguiente</button>
    <h2>Paso 3</h2>
    <label for="direccion">Dirección de residencia</label>
    <input
      type="text"
      class="form-control"
      id="direccion"
      v-model="direccion"
      @input="validarCampo('direccion', $event.target.value)"
    />
    <i v-if="errores.includes('direccion')" class="fa fa-times text-danger"></i>
    <i v-else class="fa fa-check text-success"></i>

    <label for="codigo-postal">Código Postal</label>
    <input
      type="text"
      class="form-control"
      id="codigo-postal"
      v-model="codigoPostal"
      @input="validarCampo('codigoPostal', $event.target.value)"
    />
    <i
      v-if="errores.includes('codigoPostal')"
      class="fa fa-times text-danger"
    ></i>
    <i v-else class="fa fa-check text-success"></i>
    <button @click="enviarFormulario">Enviar</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      paises: [],
      paisSeleccionado: "",
      generos: ["Masculino", "Femenino", "Otro"],

      primerNombre: "",
      segundoNombre: "",
      fechaNacimiento: "",
      tipoDocumento: "",
      numeroDocumento: "",
      fotoFrente: null,
      fotoReverso: null,
      correoElectronico: "",
      contrasena: "",
      confirmacionContrasena: "",
      numeroTelefono: "",
      numeroCelular: "",
      direccionResidencia: "",
      codigoPostal: "",
      errores: [],
    };
  },

  methods: {
    cargarPaises() {
      // Aquí debería haber una llamada a una API que descargue los países
      // y los guarde en this.paises
      fetch("https://restcountries.com/v2/all")
        .then((response) => response.json())
        .then((data) => {
          this.paises = data.map((pais) => pais.name);
        })
        .catch((error) => console.error(error));
    },
    cargarFotoFrente(evento) {
      this.fotoFrente = evento.target.files[0];
    },
    cargarFotoReverso(evento) {
      this.fotoReverso = evento.target.files[0];
    },
    validarCampo(campo, valor) {
      let correoRegex;
      let telefonoRegex;
      let celularRegex;
      let codigoPostalRegex;
      switch (campo) {
        case "correoElectronico":
          correoRegex = /^\S+@\S+\.\S+$/;
          if (!correoRegex.test(valor)) {
            this.errores.push("El correo electrónico no es válido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El correo electrónico no es válido"
            );
          }
          break;

        case "contrasena":
          this.contrasenaValida = valor.length >= 8;
          if (!this.contrasenaValida && valor !== "") {
            this.errores.push("La contraseña debe tener al menos 8 caracteres");
          } else {
            this.errores = this.errores.filter(
              (error) =>
                error !== "La contraseña debe tener al menos 8 caracteres"
            );
          }
          break;

        case "confirmarContrasena":
          this.confirmacionContrasenaValida = valor === this.contrasena;
          if (!this.confirmacionContrasenaValida && valor !== "") {
            this.errores.push("La confirmación de la contraseña no coincide");
          } else {
            this.errores = this.errores.filter(
              (error) =>
                error !== "La confirmación de la contraseña no coincide"
            );
          }
          break;

        case "numeroTelefono":
          telefonoRegex = /^\d{7,}$/;
          if (!telefonoRegex.test(valor) && valor !== "") {
            this.errores.push("El número de teléfono no es válido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El número de teléfono no es válido"
            );
          }
          break;

        case "numeroCelular":
          celularRegex = /^\d{10}$/;
          if (!celularRegex.test(valor) && valor !== "") {
            this.errores.push("El número de celular no es válido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El número de celular no es válido"
            );
          }
          break;

        case "direccionResidencia":
          if (valor.length < 10) {
            this.errores.push(
              "La dirección de residencia debe tener al menos 10 caracteres"
            );
          } else {
            this.errores = this.errores.filter(
              (error) =>
                error !==
                "La dirección de residencia debe tener al menos 10 caracteres"
            );
          }
          break;

        case "codigoPostal":
          codigoPostalRegex = /^\d{5}$/;
          if (!codigoPostalRegex.test(valor) && valor !== "") {
            this.errores.push("El código postal no es válido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El código postal no es válido"
            );
          }
          break;

        default:
          break;
      }
    },

    validarFormulario() {
      this.errores = [];

      if (!this.correoElectronicoValido) {
        this.errores.push("El correo electrónico no es válido");
      }

      if (!this.contrasenaValida) {
        this.errores.push("La contraseña debe tener al menos 8 caracteres");
      }

      if (!this.confirmacionContrasenaValida) {
        this.errores.push("La confirmación de la contraseña no coincide");
      }

      if (!this.telefonoValido && !this.celularValido) {
        this.errores.push(
          "Se requiere al menos un número de teléfono o celular"
        );
      }

      if (!this.paisSeleccionadoValido) {
        this.errores.push("Debe seleccionar un país");
      }

      if (this.errores.length === 0) {
        this.$emit("avanzar-paso");
      }
    },

    validarPais() {
      if (this.paisSeleccionado === "") {
        this.errores.push("Debe seleccionar un país");
        return false;
      }
      return true;
    },
  },

  enviarFormulario() {
    console.log("País:", this.paisSeleccionado);
    console.log("Género:", this.genero);
    console.log("Primer Nombre:", this.primerNombre);
    console.log("Segundo Nombre:", this.segundoNombre);
    console.log("Fecha de Nacimiento:", this.fechaNacimiento);
    console.log("Tipo de Documento:", this.tipoDocumento);
    console.log("Número de Documento:", this.numeroDocumento);
    console.log("Dirección Residencia:", this.direccionResidencia);
    console.log("Código Postal:", this.codigoPostal);

    // mostrar el modal de confirmación
    alert("Formulario enviado con éxito");

    // también puedes reiniciar las propiedades del formulario aquí
  },

  created() {
    this.cargarPaises();
  },
};
</script>
