<template>
  <h3>Registro</h3>

  <div class="q-pa-md" style="max-width: 400px">
    <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
      <q-input
        filled
        v-model="Namevalue"
        label="Nombre*"
        hint="Nombre"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, llenar este campo',
        ]"
      />

      <q-input
        filled
        v-model="Lastnamevalue"
        label="Apellido*"
        hint="Apellido"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, llenar este campo',
        ]"
      />

      <q-input
        filled
        type="date"
        v-model="Datevalue"
        label="Fecha Nacimiento*"
        hint="Fecha Nacimiento"
        lazy-rules
        :rules="[
          (val) => !!val || 'Por favor, ingresa una fecha de nacimiento',
          (val) => {
            const today = new Date();
            const selectedDate = new Date(val);
            return (
              selectedDate < today ||
              'La fecha de nacimiento no puede ser futura'
            );
          },
          (val) => {
            const minAge = 18;
            const today = new Date();
            const selectedDate = new Date(val);
            const age = today.getFullYear() - selectedDate.getFullYear();
            const monthDifference = today.getMonth() - selectedDate.getMonth();
            if (
              monthDifference < 0 ||
              (monthDifference === 0 &&
                today.getDate() < selectedDate.getDate())
            ) {
              return age - 1 >= minAge || `Debes tener al menos ${minAge} años`;
            }
            return age >= minAge || `Debes tener al menos ${minAge} años`;
          },
        ]"
      />

      <q-input
        filled
        v-model="Countryvalue"
        label="País*"
        hint="País"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, llenar este campo',
        ]"
      />

      <q-input
        filled
        v-model="Addressvalue"
        label="Dirección*"
        hint="Dirección"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, llenar este campo',
        ]"
      />

      <q-input
        filled
        v-model="Emailvalue"
        label="Correo electrónico*"
        hint="email"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, llenar este campo',
          (val) => {
            const emailPattern =
              /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
            return (
              emailPattern.test(val) || 'Por favor, ingresa un correo válido'
            );
          },
        ]"
      />

      <q-input
        filled
        v-model="Passwordvalue"
        label="Contraseña*"
        hint="Contraseña"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type something']"
      />

      <div>
        <q-btn
          label="Registrar"
          type="submit"
          color="primary"
          @click="registroUsuario"
        />
        <q-btn
          label="Limpiar"
          type="reset"
          color="primary"
          flat
          class="q-ml-sm"
          @click="limpiarFormulario"
        />
      </div>
    </q-form>
  </div>
</template>

<style></style>

<script>
export default {
  name: "RegisterForm",
  data() {
    return {
      Namevalue: "",
      Lastnamevalue: "",
      Datevalue: "",
      Countryvalue: "",
      Addressvalue: "",
      Emailvalue: "",
      Passwordvalue: "",
    };
  },
  methods: {
    registroUsuario() {
      console.log("Click en el botón de registro de usuario");
      console.log("Nombre: " + this.Namevalue);
      console.log("Apellido: " + this.Lastnamevalue);
      console.log("Fecha de nacimiento: " + this.Datevalue);
      console.log("País: " + this.Countryvalue);
      console.log("Dirección: " + this.Addressvalue);
      console.log("Correo electrónico: " + this.Emailvalue);

      let endpointSignUp = "/api/User/SignUp";
      let newUser = {
        firstName: this.Namevalue,
        lastName: this.Lastnamevalue,
        dateOfBirth: this.Datevalue,
        country: this.Countryvalue,
        address: this.Addressvalue,
        email: this.Emailvalue,
        password: this.Passwordvalue,
      };

      this.$api
        .post(endpointSignUp, newUser)
        .then((response) => {
          // Respuesta exitosa
          console.log("Registro exitoso: " + JSON.stringify(response));
          this.$q.notify({
            message: "Registro exitoso. Bienvenido a Store APP",
            color: "positive",
            position: "bottom",
            timeout: 5000,
          });

          // Redirigir a la página de inicio de sesión o a otra página
          this.$router.push("/login");
        })
        .catch((error) => {
          // Ocurrió un error en el registro
          this.$q.notify({
            message: "Ocurrió un error al registrarse",
            color: "negative",
            position: "top",
            timeout: 5000,
          });
          console.log("Error en el registro: " + error);
        });
    },
    limpiarFormulario() {
      // Restablecer todos los campos del formulario a valores vacíos
      this.Namevalue = "";
      this.Lastnamevalue = "";
      this.Datevalue = "";
      this.Countryvalue = "";
      this.Emailvalue = "";
      this.Passwordvalue = "";

      console.log("Formulario limpiado");
    },
  },
};
</script>
