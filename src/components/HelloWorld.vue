<template>
  <form @submit.prevent="submitForm">
    <div class="container">
      <div v-if="pasoActual === 1">
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
        <h3 v-if="errores.length">&times;</h3>
        <h3 v-else>&#10003;</h3>
        <input
          type="text"
          id="primer-nombre"
          v-model="primerNombre"
          @input="validarCampo('primerNombre', $event.target.value)"
          required
        />

        <label for="segundo-nombre">Apellido</label>
        <input
          type="text"
          id="primer-nombre"
          v-model="segundoNombre"
          @input="validarCampo('primerNombre', $event.target.value)"
          required
        />

        <label for="fecha-nacimiento">Fecha de Nacimiento</label>
        <input type="date" id="fecha-nacimiento" v-model="fechaNacimiento" />

        <label for="tipo-documento">Tipo de Documento</label>
        <select id="tipo-documento" v-model="tipoDocumento">
          <option value="cedula">Cedula de ciudadanía</option>
          <option value="pasaporte">Pasaporte</option>
          <option value="cedula-extranjeria">Cedula de extranjería</option>
        </select>

        <label for="numero-documento">Número de Documento</label>
        <input type="number" id="numero-documento" v-model="numeroDocumento" />

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
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
          @click="validarFormularioYAvanzar"
        >
          Siguiente
        </button>
      </div>

      <div v-if="pasoActual === 2">
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
        <i
          v-if="errores.includes('telefono')"
          class="fa fa-times text-danger"
        ></i>
        <i v-else class="fa fa-check text-success"></i>

        <label for="celular">Celular</label>
        <input
          type="tel"
          class="form-control"
          id="celular"
          v-model="celular"
          @input="validarCampo('celular', $event.target.value)"
        />
        <i
          v-if="errores.includes('celular')"
          class="fa fa-times text-danger"
        ></i>
        <i v-else class="fa fa-check text-success"></i>

        <div v-if="errores.length">
          <h3>Errores de Validación</h3>
          <ul>
            <li v-for="error in errores" :key="error">{{ error }}</li>
          </ul>
        </div>
        <button @click="retrocederPaso">Retroceder</button>
        <button @click="validarFormularioYAvanzar">Siguiente</button>
      </div>
      <div v-if="pasoActual === 3">
        <h2>Paso 3</h2>
        <label for="direccion">Dirección de residencia</label>
        <input
          type="text"
          class="form-control"
          id="direccion"
          v-model="direccion"
          @input="validarCampo('direccion', $event.target.value)"
        />
        <i
          v-if="errores.includes('direccion')"
          class="fa fa-times text-danger"
        ></i>
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
        <button @click="retrocederPaso">Retroceder</button>
        <button @click="enviarFormulario">Enviar</button>
      </div>
    </div>
  </form>

  <div
    class="fixed z-50 inset-0 overflow-y-auto"
    :class="{ hidden: !showModal }"
  >
    <div class="flex items-center justify-center min-h-screen">
      <div class="fixed inset-0 bg-gray-800 opacity-75"></div>
      <div class="bg-white rounded-lg p-8 z-50">
        <div class="mb-4">
          <h3 class="text-lg font-medium text-gray-900">Enviado con éxito</h3>
          <p class="text-gray-600">
            Tu formulario ha sido enviado correctamente.
          </p>
        </div>
        <div class="flex justify-end">
          <button
            @click="showModal = false"
            class="bg-gray-700 text-white px-4 py-2 rounded-lg"
          >
            Cerrar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      paises: [],
      paisSeleccionado: "",
      generos: ["Masculino", "Femenino", "Otro"],
      genero: "",
      pasoActual: 1,
      campo1: "",
      campo2: "",
      campo3: "",
      campo1Valido: false,
      campo2Valido: false,
      campo3Valido: false,
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
    validarFormularioYAvanzar() {
      let formularioValido = true; // Comenzar asumiendo que el formulario es válido
      const camposRequeridos = document.querySelectorAll("[required]"); // Seleccionar todos los campos requeridos

      // Recorrer todos los campos requeridos y verificar si han sido completados
      camposRequeridos.forEach((campo) => {
        if (!campo.value.trim()) {
          // Si el valor está en blanco, marcar como no válido
          formularioValido = false;
        }
      });

      // Si el formulario es válido, permitir el avance al siguiente paso
      if (formularioValido) {
        this.avanzarPaso();
      }
    },

    avanzarPaso() {
      this.pasoActual += 1;
    },
    retrocederPaso() {
      this.pasoActual -= 1;
    },
    validarCampo1() {
      this.campo1Valido = this.campo1.trim() !== "";
      return this.campo1Valido;
    },
    validarCampo2() {
      this.campo2Valido = this.campo2.trim() !== "";
      return this.campo2Valido;
    },
    validarCampo3() {
      this.campo3Valido = this.campo3.trim() !== "";
      return this.campo3Valido;
    },
    validarCampos() {
      return (
        this.validarCampo1() && this.validarCampo2() && this.validarCampo3()
      );
    },
    enviarFormulario() {
      console.log("País:", this.paisSeleccionado);
      console.log("Género:", this.genero);
      console.log("Primer Nombre:", this.primerNombre);
      console.log("Segundo Nombre:", this.segundoNombre);
      console.log("Fecha de Nacimiento:", this.fechaNacimiento);
      console.log("Tipo de Documento:", this.tipoDocumento);
      console.log("Número de Documento:", this.numeroDocumento);
      console.log("Correo Electronico:", this.correoElectronico);
      this.contrasena = "*********";
      console.log("Contraseña:", this.contrasena);
      console.log("Dirección Residencia:", this.direccionResidencia);
      console.log("Código Postal:", this.codigoPostal);

      // mostrar el modal de confirmación
      //alert("Formulario enviado con éxito");
      this.showModal = true;

      // también puedes reiniciar las propiedades del formulario aquí
    },

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
        case "primerNombre":
          if (valor.trim() === "") {
            this.errores.push("El primer nombre es requerido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El primer nombre es requerido"
            );
          }
          break;

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

      if (!this.primerNombre) {
        this.errores.push("El nombre esta vacio");
      }

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

  created() {
    this.cargarPaises();
  },
};
</script>

<style>
.icono-validacion {
  font-size: 1rem;
  margin-left: 0.5rem;
}

.icono-validacion::before {
  display: inline-block;
  font-family: "Font Awesome 5 Free";
  font-weight: 900;
}

.icono-validacion::before {
  content: "\f00c";
  color: green;
}

.icono-validacion:not([v-if])::before {
  content: "\f00d";
  color: red;
}
</style>
