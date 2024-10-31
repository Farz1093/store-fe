<template>
  <q-layout>
    <q-page-container>
      <q-page class="flex flex-center q-pa-md" style="height: 100vh">
        <q-card-section>
          <h5>Inicio de sesión</h5>
        </q-card-section>
        <q-card-section>
          <q-input
            v-model="emailValue"
            standout="bg-teal text-white"
            label="Email"
            outlined
            dense
          />
          <q-input
            v-model="pwdValue"
            standout="bg-teal text-white"
            label="Password"
            outlined
            dense
          />
        </q-card-section>
        <q-card-section>
          <q-btn label="Login" color="primary" @click="inicioSesion" />
        </q-card-section>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<style></style>

<script>
export default {
  name: "LoginForm",
  data() {
    return {
      emailValue: "",
      pwdValue: "",
    };
  },
  methods: {
    inicioSesion() {
      console.log("Click en el boton de inici de sesión");
      console.log("Valor del email: " + this.emailValue);

      let endpointLogin = "/api/User/SignIn";
      let user = { email: this.emailValue, password: this.pwdValue };

      this.$api
        .post(endpointLogin, user)
        .then((response) => {
          //respuesta exitosa
          console.log("Respuesta exitosa: " + JSON.stringify(response));
          this.$q.notify({
            message: "Bienvenido a Store APP",
            color: "positive",
            position: "bottom",
            timeout: 5000,
          });

          this.$router.push("/");
        })
        .catch((error) => {
          //Ocurrió un error
          this.$q.notify({
            message: "Ocurrió un error",
            color: "negative",
            position: "top",
            timeout: 5000,
          });
          console.log("Error en: " + error);
        });
    },
  },
};
</script>
