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
          type="number"
          class="form-control"
          :class="{ 'is-invalid': v$.user.phone.$error }"
        />
        <div class="invalid-feedback">Telefono deben ser solo numeros</div>
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
import { onMounted } from "vue";
import { reactive, computed } from "vue";
import useVuelidate from "@vuelidate/core";
import { required, numeric, email } from "@vuelidate/validators";
// export default {
//   name: "NuevoUser",
//   data() {
//     return {
//       title: "NewUser",
//       user: reactive({
//         id: "",
//         name: "",
//         email: "",
//         address: "",
//         phone: "",
//         country: "",
//         city: "",
//       }),
//       v$: null,
//     };
//   },
//   components: {},
//   created() {
//     const rules = {
//       user: {
//         name: { required },
//         email: { required },
//         address: { required },
//         phone: { required },
//         country: { required },
//         city: { required },
//       },
//     };
//     this.v$ = useVuelidate(rules, { user: this.user });
//   },
//   methods: {
//     async submit() {
//       const isValid = await this.v$.$validate();
//       if (!isValid) {
//         alert("formulario con errores");
//         return;
//       }
//       this.$emit("created", { ...this.user });
//     },
//   },
//   emits: ["created"],
//   computed: {},
//   props: {},
//   emits: [],
// };
const title = "Registro de nuevo usuario";
const user = reactive({
  name: "",
  email: "",
  address: "",
  phone: "",
  country: "",
  city: "",
});
const emit = defineEmits(["created"]);
const rules = computed(() => {
  return {
    user: {
      name: { required },
      email: { required },
      address: { required },
      phone: { required },
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
};
</script>
