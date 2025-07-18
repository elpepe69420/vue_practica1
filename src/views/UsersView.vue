<template>
  <h1>{{ title }}</h1>
  <p>
    <button type="button" class="btn btn-primary" @click="goToNew()">
      Nuevo
    </button>
  </p>
  <div class="mb-3">
    <div class="input-group">
      <select name="campo" id="campo" v-model="busqueda">
        <option value="0">Sin filtro</option>
        <option value="1">Nombre</option>
        <option value="2">Correo</option>
      </select>
      <div v-show="busqueda == 1 || busqueda == 2" class="input-group">
        <span class="input-group-text" id="basic-addon3"
          >Buscar por
          {{ busqueda == 1 ? "nombre" : busqueda == 2 ? "correo" : "" }}</span
        >
        <input type="search" v-model="filtroBuscar" class="form-control" />
      </div>
    </div>
  </div>
  <div>
    <table class="table table-bordered border-primary">
      <thead>
        <tr>
          <th scope="col">id</th>
          <th scope="col">Nombre</th>
          <th scope="col">Correo</th>
          <th scope="col">Direccion</th>
          <th scope="col">Telefono</th>
          <th scope="col">Pais</th>
          <th scope="col">Ciudad</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in getItems()" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.address }}</td>
          <td>{{ item.phone }}</td>
          <td>{{ item.country }}</td>
          <td>{{ item.city }}</td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              @click="abrirModal(index)"
            >
              Editar
            </button>
            <button
              type="button"
              class="btn btn-danger"
              @click="eliminar(index)"
            >
              eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Modal Bootstrap -->
  <div
    class="modal fade"
    id="modalAutoEditar"
    tabindex="-1"
    aria-labelledby="modalAutoEditarLabel"
    aria-hidden="true"
    ref="modalRef"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="modalAutoEditarLabel">Editar Usuario</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Cerrar"
          ></button>
        </div>
        <div class="modal-body">
          <!-- Componente AutoEditar -->
          <EditarUser
            v-if="modalMode == 'editar' && userSeleccionado"
            :user="userSeleccionado"
            @update="guardarEdicion"
            @cancelar="cerrarModal"
          />
          <NuevoUser
            v-if="modalMode == 'crear'"
            @created="agregarNuevo($event)"
          ></NuevoUser>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NuevoUser from "@/components/NuevoUser.vue";
import EditarUser from "@/components/EditarUser.vue";
import usersData from "@/json/db.json";
export default {
  name: "UsersView",
  data() {
    return {
      title: "Usuarios",
      items: usersData,
      busqueda: 0,
      modalBootstrapInstance: null,
      userSeleccionado: null,
      indiceSeleccionado: 0,
      nameSeleccionado: "",
      filtroBuscar: "",
      modalMode: "crear",
    };
  },
  components: {
    NuevoUser,
    EditarUser,
  },
  mounted() {
    this.$nextTick(() => {
      if (this.$refs.modalRef) {
        this.modalBootstrapInstance = new bootstrap.Modal(this.$refs.modalRef);
      } else {
        console.error("no se encontro el modalref");
      }
    });
  },
  methods: {
    goToNew() {
      this.modalMode = "crear";
      if (this.modalBootstrapInstance) {
        this.modalBootstrapInstance.show();
      } else {
        console.error("modalBootstrapInstance no está inicializado gotonew");
      }
    },
    abrirModal(index) {
      this.autoSeleccionado = null;
      this.indiceSeleccionado = index;
      this.modalMode = "editar";
      setTimeout(() => {
        if (this.modalBootstrapInstance) {
          this.modalBootstrapInstance.show();
          this.userSeleccionado = { ...this.items[index] };
        } else {
          console.error("modalBootstrapInstance no está inicializado");
        }
      });
    },
    cerrarModal() {
      if (this.modalBootstrapInstance) {
        this.modalBootstrapInstance.hide();
      }
    },
    guardarEdicion(userEditado) {
      console.log("User editado:", userEditado);
      // Aquí actualizas la info, haces llamada a API, etc.
      this.items[this.indiceSeleccionado] = userEditado;
      this.cerrarModal();
    },

    eliminar(index) {
      if (confirm("¿Está seguro de eliminar este auto?")) {
        this.items.splice(index, 1);
      }
    },
    agregarNuevo($event) {
      const maxId = Math.max(...this.items.map((user) => user.id));
      $event["id"] = maxId + 1;
      this.items.push($event);
      console.log($event);
      this.cerrarModal();
    },
    getItems() {
      let result = [...this.items];
      if (this.busqueda == 1) {
        if (this.filtroBuscar !== "") {
          result = this.items.filter((item) =>
            item.name.includes(this.filtroBuscar)
          );
        }
      } else if (this.busqueda == 2) {
        if (this.filtroBuscar !== "") {
          result = this.items.filter((item) =>
            item.email.includes(this.filtroBuscar)
          );
        }
      }
      return result;
    },
  },
  computed: {
    startItems() {
      this.getItems();
    },
  },
};
</script>
