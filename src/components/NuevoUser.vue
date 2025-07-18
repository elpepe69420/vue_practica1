<template>
  <h1>{{ title }}</h1>

  <div class="container mt-4">
    <h2 class="mb-4">Registrar nuevo usuario</h2>

    <form @submit.prevent="submit" novalidate>
      <div class="mb-3">
        <label class="form-label">Nombre</label>
        <input
          v-model="user.name"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': v$.user.name.$error }"
        />
        <div class="invalid-feedback">Nombre es obligatoria</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Correo</label>
        <input
          v-model="user.email"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': v$.user.email.$error }"
        />
        <div class="invalid-feedback">Correo es obligatorio</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Direccion</label>
        <input
          v-model="user.address"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': v$.user.address.$error }"
        />
        <div class="invalid-feedback">Direccion es obligatorio</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Telefono</label>
        <input
          v-model="user.phone"
          placeholder="XXX-YYY-ZZZZ"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': v$.user.phone.$error }"
        />
        <div class="invalid-feedback">
          Telefono debe ser de acuerdo al patron
        </div>
      </div>

      <div class="mb-3">
        <label class="form-label">Pais</label>
        <input
          v-model="user.country"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': v$.user.country.$error }"
        />
        <div class="invalid-feedback">Pais es obligatorio</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Ciudad</label>
        <input
          v-model="user.city"
          type="text"
          class="form-select"
          :class="{ 'is-invalid': v$.user.city.$error }"
        />
        <div class="invalid-feedback">Ciudad es obligatorio</div>
      </div>

      <button type="submit" class="btn btn-primary">Guardar</button>
    </form>
  </div>
</template>

<script setup>
import { reactive, computed } from "vue";
import { helpers } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";

const title = "Registro de nuevo usuario";
const phoneValidatorCustom = helpers.regex(
  /^(\+0?1\s)?\(?\d{3}\)?[\s.-]\d{3}[\s.-]\d{4}$/
);
const user = reactive({
  name: "",
  email: "",
  address: "",
  phone: "",
  country: "",
  city: "",
});
const borrarForm = () => {
  user.name = "";
  user.email = "";
  user.address = "";
  user.phone = "";
  user.country = "";
  user.city = "";
};
const emit = defineEmits(["created"]);
const rules = computed(() => {
  return {
    user: {
      name: { required },
      email: { required, email },
      address: { required },
      phone: { required, phoneValidatorCustom },
      country: { required },
      city: { required },
    },
  };
});
const v$ = useVuelidate(rules, { user: user }, { $autoDirty: true });

const submit = async () => {
  const isValid = await v$.value.$validate();
  if (!isValid) {
    alert("formulario con errores");
    return;
  }
  emit("created", { ...user });
  borrarForm();
};
</script>
