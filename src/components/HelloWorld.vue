<template>
  <form @submit.prevent="submitForm">
    <div class="container">
      <div v-if="pasoActual === 1">
        <h2 class="font-bold text-3xl mb-4">Paso 1</h2>

        <label for="pais" class="block font-bold mb-2">País</label>
        <select
          id="pais"
          v-model="paisSeleccionado"
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          required
        >
          <option v-for="pais in paises" :value="pais" :key="pais">
            {{ pais }}
          </option>
        </select>

        <label for="genero" class="block font-bold mb-2">Género</label>
        <select
          id="genero"
          v-model="genero"
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          required
        >
          <option v-for="genero in generos" :value="genero" :key="genero">
            {{ genero }}
          </option>
        </select>

        <label for="primer-nombre" class="block font-bold mb-2"
          >Primer Nombre</label
        >

        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          type="text"
          id="primer-nombre"
          v-model="primerNombre"
          @input="validarCampo('primerNombre', $event.target.value)"
          required
        />

        <label for="segundo-nombre" class="block font-bold mb-2"
          >Segundo Nombre</label
        >

        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          type="text"
          id="segundo-nombre"
          v-model="segundoNombre"
          @input="validarCampo('segundoNombre', $event.target.value)"
          required
        />

        <label for="fecha-nacimiento" class="block font-bold mb-2"
          >Fecha de Nacimiento</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          type="date"
          id="fecha-nacimiento"
          v-model="fechaNacimiento"
          required
        />

        <label for="tipo-documento" class="block font-bold mb-2"
          >Tipo de Documento</label
        >
        <select
          id="tipo-documento"
          v-model="tipoDocumento"
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          required
        >
          <option value="cedula">Cedula de ciudadanía</option>
          <option value="pasaporte">Pasaporte</option>
          <option value="cedula-extranjeria">Cedula de extranjería</option>
        </select>

        <label for="numero-documento" class="block font-bold mb-2"
          >Número de Documento</label
        >
        <input
          type="number"
          id="numero-documento"
          v-model="numeroDocumento"
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          required
        />

        <label for="foto-frente" class="block font-bold mb-2"
          >Foto del Documento - Frente</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          type="file"
          id="foto-frente"
          accept=".jpg"
          @change="cargarFotoFrente"
          required
        />

        <label for="foto-reverso" class="block font-bold mb-2"
          >Foto del Documento - Reverso</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4"
          type="file"
          id="foto-reverso"
          accept=".jpg"
          @change="cargarFotoReverso"
          required
        />

        <button
          class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded font-bold cursor-pointer"
          @click="validarFormularioYAvanzar"
        >
          Siguiente
        </button>
      </div>

      <div v-if="pasoActual === 2">
        <h2 class="font-bold text-3xl mb-4">Paso 2</h2>
        <label for="correo-electronico" class="block font-bold mb-2"
          >Correo electrónico</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="email"
          id="correo-electronico"
          v-model="correoElectronico"
          @input="validarCampo('correoElectronico', $event.target.value)"
          required
        />

        <label for="contrasena" class="block font-bold mb-2">Contraseña</label>
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="password"
          id="contrasena"
          v-model="contrasena"
          @input="validarCampo('contrasena', $event.target.value)"
          required
        />

        <label for="confirmar-contrasena" class="block font-bold mb-2"
          >Confirmar contraseña</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="password"
          id="confirmar-contrasena"
          v-model="confirmarContrasena"
          @input="validarCampo('confirmarContrasena', $event.target.value)"
          required
        />

        <label for="telefono" class="block font-bold mb-2">Teléfono</label>
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="tel"
          id="telefono"
          v-model="telefono"
          @input="validarCampo('telefono', $event.target.value)"
          required
        />

        <label for="celular" class="block font-bold mb-2">Celular</label>
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="tel"
          id="celular"
          v-model="celular"
          @input="validarCampo('celular', $event.target.value)"
          required
        />

        <div class="flex justify-between">
          <button
            class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded font-bold cursor-pointer"
            @click="retrocederPaso"
          >
            Retroceder
          </button>
          <button
            class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded font-bold cursor-pointer"
            @click="validarFormularioYAvanzar"
          >
            Siguiente
          </button>
        </div>
      </div>
      <div v-if="pasoActual === 3">
        <h2 class="font-bold text-3xl mb-4">Paso 3</h2>
        <label for="direccion" class="block font-bold mb-2"
          >Dirección de residencia</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="text"
          id="direccion"
          v-model="direccion"
          @input="validarCampo('direccion', $event.target.value)"
          required
        />

        <label for="codigo-postal" class="block font-bold mb-2"
          >Código Postal</label
        >
        <input
          class="w-full py-2 px-4 border border-gray-300 rounded mb-4 form-control"
          type="text"
          id="codigo-postal"
          v-model="codigoPostal"
          @input="validarCampo('codigoPostal', $event.target.value)"
          required
        />
        <div class="flex justify-between">
          <button
            class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded font-bold cursor-pointer"
            @click="retrocederPaso"
          >
            Retroceder
          </button>
          <button
            class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded font-bold cursor-pointer"
            @click="enviarFormulario"
          >
            Enviar
          </button>
        </div>
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

      primerNombre: "",
      segundoNombre: "",
      fechaNacimiento: "",
      tipoDocumento: "",
      numeroDocumento: "",
      fotoFrente: null,
      fotoReverso: null,
      correoElectronico: "",
      contrasena: "",
      confirmarContrasena: "",
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

    enviarFormulario() {
      console.log("País:", this.paisSeleccionado);
      console.log("Género:", this.genero);
      console.log("Primer Nombre:", this.primerNombre);
      console.log("Segundo Nombre:", this.segundoNombre);
      console.log("Fecha de Nacimiento:", this.fechaNacimiento);
      console.log("Tipo de Documento:", this.tipoDocumento);
      console.log("Número de Documento:", this.numeroDocumento);

      console.log("Correo Electronico:", this.correoElectronico);
      this.contrasena = "••••••••••";
      console.log("Contraseña:", this.contrasena);
      console.log("Telefono:", this.telefono);
      console.log("Celular:", this.celular);
      console.log("Dirección Residencia:", this.direccion);
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
      let numeroDocumentoRegex = /^\d{5,}$/;
      switch (campo) {
        case "numeroDocumento":
          if (!numeroDocumentoRegex.test(valor)) {
            this.errores.push("El número de documento no es válido");
          } else {
            this.errores = this.errores.filter(
              (error) => error !== "El número de documento no es válido"
            );
          }
          break;

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
  },

  created() {
    this.cargarPaises();
  },
};
</script>
