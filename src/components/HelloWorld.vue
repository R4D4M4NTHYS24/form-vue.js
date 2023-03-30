<template>
  <div>
    <h2>Paso 1</h2>
    <label for="pais">País</label>
    <select id="pais" v-model="paisSeleccionado">
      <option v-for="pais in paises" :value="pais" :key="pais">
        {{ pais }}
      </option>
    </select>

    <label for="genero">Género</label>
    <input type="text" id="genero" v-model="genero" />

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

    <button @click="avanzarPaso">Siguiente</button>
  </div>
  <h2>Paso 2</h2>
  <label for="correo-electronico">Correo Electrónico</label>
  <input type="email" id="correo-electronico" v-model="correoElectronico" />

  <label for="contrasena">Contraseña</label>
  <input type="password" id="contrasena" v-model="contrasena" />

  <label for="confirmacion-contrasena">Confirmación de Contraseña</label>
  <input
    type="password"
    id="confirmacion-contrasena"
    v-model="confirmacionContrasena"
  />

  <label for="numero-telefono">Número de Teléfono</label>
  <input type="tel" id="numero-telefono" v-model="numeroTelefono" />

  <label for="numero-celular">Número de Celular</label>
  <input type="tel" id="numero-celular" v-model="numeroCelular" />

  <div v-if="errores.length">
    <h3>Errores de Validación</h3>
    <ul>
      <li v-for="error in errores" :key="error">{{ error }}</li>
    </ul>
  </div>

  <button @click="avanzarPaso">Siguiente</button>
  <h2>Paso 3</h2>
  <label for="direccion-residencia">Dirección Residencia</label>
  <input type="text" id="direccion-residencia" v-model="direccionResidencia" />

  <label for="codigo-postal">Código Postal</label>
  <input type="text" id="codigo-postal" v-model="codigoPostal" />
  <button @click="enviarFormulario">Enviar</button>
</template>

<script>
export default {
  data() {
    return {
      paises: [],
      paisSeleccionado: "",
      genero: "",
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
    },
    cargarFotoFrente(evento) {
      this.fotoFrente = evento.target.files[0];
    },
    cargarFotoReverso(evento) {
      this.fotoReverso = evento.target.files[0];
    },
    validarFormulario() {
      this.errores = [];

      if (!this.correoElectronico) {
        this.errores.push("El correo electrónico es obligatorio");
      }

      if (!this.contrasena) {
        this.errores.push("La contraseña es obligatoria");
      }

      if (this.contrasena !== this.confirmacionContrasena) {
        this.errores.push("La confirmación de la contraseña no coincide");
      }

      if (!this.numeroTelefono && !this.numeroCelular) {
        this.errores.push(
          "Se requiere al menos un número de teléfono o celular"
        );
      }

      if (this.errores.length === 0) {
        this.$emit("avanzar-paso");
      }
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
