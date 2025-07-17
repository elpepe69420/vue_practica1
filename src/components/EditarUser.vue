<template>
  <div class="container mt-4">
    <h2 class="mb-4">Registrar nuevo usuario</h2>

    <form @submit.prevent="emitirDatosEditados" novalidate>
      <div class="mb-3">
        <label class="form-label">Nombre</label>
        <input v-model="edited.name" type="text" class="form-control" />
      </div>

      <div class="mb-3">
        <label class="form-label">Correo</label>
        <input v-model="edited.email" type="text" class="form-control" />
      </div>

      <div class="mb-3">
        <label class="form-label">Direccion</label>
        <input v-model="edited.address" type="text" class="form-control" />
      </div>

      <div class="mb-3">
        <label class="form-label">Telefono</label>
        <input v-model="edited.phone" type="text" class="form-control" />
      </div>

      <div class="mb-3">
        <label class="form-label">Pais</label>
        <input v-model="edited.country" type="text" class="form-control" />
      </div>

      <div class="mb-3">
        <label class="form-label">Ciudad</label>
        <input v-model="edited.city" type="text" class="form-select" />
      </div>

      <button type="submit" class="btn btn-primary">Guardar cambios</button>
    </form>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";
import useVuelidate from "@vuelidate/core";
import { required, numeric, email } from "@vuelidate/validators";
// export default {
//   name: "EditarUser",
//   props: {
//     user: {
//       type: Object,
//       required: true,
//     },
//   },
//   created() {
//     // const rules = {
//     //   edited: {
//     //     name: { required },
//     //     email: { required, email },
//     //     address: { required },
//     //     phone: { required, numeric },
//     //     country: { required },
//     //     city: { required },
//     //   },
//     // };
//     // this.v$ = useVuelidate(rules, { user: edited });
//   },
//   data() {
//     return {
//       edited: { ...this.user },
//     };
//   },
//   methods: {
//     emitirDatosEditados() {
//       console.log("paso1");
//       this.$emit("update", this.edited);
//     },
//   },
//   emits: ["update"],
// };
const props = defineProps({
  user: Object,
  required: true,
});
const edited = { ...props.user };
const emit = defineEmits(["update"]);
const emitirDatosEditados = () => {
  emit("update", edited);
};
</script>
