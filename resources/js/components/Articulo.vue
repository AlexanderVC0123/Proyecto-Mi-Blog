<template>
  <div>
    <h1 class="text-center">Gestionar Artículos</h1>
    <hr />

    <button
      @click="
        modificar = false;
        abrirModal();
      "
      type="button"
      class="btn btn-primary my-4"
    >
      Nuevo artículo
    </button>

    <div class="modal" :class="{ mostrar: modal }">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ tituloModal }}</h5>
            <button
              @click="cerrarModal()"
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            >
              &times;
            </button>
          </div>
          <div class="modal-body">
            <div class="my-4">
              <label for="nombre">Nombre</label>
              <input
                v-model="articulo.nombre"
                type="text"
                class="form-control"
                id="nombre"
                placeholder="Nombre del articulo"
              />
            </div>
            <div class="my-4">
              <label for="descripcion">Descripcion</label>
              <input
                v-model="articulo.descripcion"
                type="text"
                class="form-control"
                id="descripcion"
                placeholder="Escribe tu articulo aquí"
              />
            </div>
            <div class="my-4">
              {{ articulo.fecha }}
            </div>
          </div>
          <div class="modal-footer">
            <button
              @click="cerrarModal()"
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cancelar
            </button>
            <button
                @click="guardar()"
              type="button"
              class="btn btn-success"
              data-bs-dismiss="modal"
            >
              Guardar
            </button>
          </div>
        </div>
      </div>
    </div>

    <table class="table table-striped table-hover">
      <thead class="table-dark">
        <tr>
          <th scope="col">Id</th>
          <th scope="col">Titulo</th>
          <th scope="col">Descripción</th>
          <th scope="col">Fecha</th>
          <th scope="col" colspan="2" class="text-center"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="art in articulos" :key="art.id">
          <th scope="row">{{ art.id }}</th>
          <td>{{ art.nameArticle }}</td>
          <td>{{ art.description }}</td>
          <td>{{ art.date }}</td>
          <td>
            <button
              @click="
                modificar = true;
                abrirModal(art);
              "
              class="btn btn-warning"
            >
              Editar
            </button>
          </td>
          <td>
            <button @click="eliminar(art.id)" class="btn btn-danger">
              Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      articulo: {
        nombre: '',
        descripcion: '',
        fecha: "23/02/2021",
      },
      id: 0,
      modificar: true,
      modal: 0,
      tituloModal: "",
      articulos: [],
    };
  },

  methods: {
    async listar() {
      const res = await axios.get("articulos");
      this.articulos = res.data;
    },

    async eliminar(id) {
      const res = await axios.delete("articulos/" + id);
      this.listar();
    },

    async guardar() {

        if(this.modificar){
            
            const res = await axios.put("/articulos/"+this.id, this.articulo);


        }else{
            const res = await axios.post("/articulos", this.articulo);
        }
        this.cerrarModal();
        this.listar();
    },


    abrirModal(data = {}) {
      this.modal = 1;
      if (this.modificar) {
        this.id = data.id;
        this.tituloModal = "Modificar articulo";
        this.articulo.nombre = data.nameArticle;
        this.articulo.descripcion = data.description;
      } else {
          this.id = 0;
        this.tituloModal = "Crear articulo";
        this.articulo.nombre = "";
        this.articulo.descripcion = "";
      }
    },
    cerrarModal() {
      this.modal = 0;
    },
  },

  created() {
    this.listar();
  },
};
</script>

<style>
.mostrar {
  display: list-item;
  opacity: 1;
  background: grey;
}
</style>