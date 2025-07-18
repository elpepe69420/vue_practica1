<template>
  <div class="container mt-4">
    <h2 class="mb-4">Registrar nuevo usuario</h2>

    <form @submit.prevent="emitirDatosEditados" novalidate>
      <div class="mb-3">
        <label class="form-label">Nombre</label>
        <input
          v-model="edited.name"
          type="text"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label class="form-label">Correo</label>
        <input
          v-model="edited.email"
          type="text"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label class="form-label">Direccion</label>
        <input
          v-model="edited.address"
          type="text"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label class="form-label">Telefono</label>
        <input
          v-model="edited.phone"
          type="text"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label class="form-label">Pais</label>
        <input
          v-model="edited.country"
          type="text"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label class="form-label">Ciudad</label>
        <input v-model="edited.city" type="text" class="form-select" required />
      </div>

      <button type="submit" class="btn btn-primary">Guardar cambios</button>
    </form>
  </div>
</template>

<script setup>
import { defineEmits, watchEffect } from "vue";

const props = defineProps({
  user: Object,
  required: true,
});
let edited = null;
watchEffect(() => {
  edited = props.user;
});
const emit = defineEmits(["update"]);
const emitirDatosEditados = () => {
  emit("update", edited);
};
</script>
