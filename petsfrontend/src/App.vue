<template>
  <div>
    <table>
      <tr>
        <th>Id</th>
        <th>Categoría</th>
        <th>Nombre</th>
        <th>Url de fotos</th>
        <th>Tags</th>
        <th>Status</th>
      </tr>
      <tr v-for="(date, index) in data" v-bind:key="index">
        <td>{{ date.id }}</td>
        <td>{{ date.category }}</td>
        <td>{{ date.name }}</td>
        <td>
          <tr v-for="(date, index) in date.photoUrls" v-bind:key="index">
            <td>{{ date }}</td>
          </tr>
        </td>

        <td>
          <tr v-for="(date, index) in date.tags" v-bind:key="index">
            <td>{{ date }}</td>
          </tr>
        </td>

        <td>{{ date.status }}</td>
      </tr>
    </table>

    <button type="button" class="btn btn-primary" @click="toggleModal">
      My Modal
    </button>
    <div
      ref="modal"
      class="modal fade"
      :class="{ show, 'd-block': active }"
      tabindex="-1"
      role="dialog"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Modal title</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
              @click="toggleModal"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <label for="category">Categoría</label><br />
              <input
                type="text"
                @change="category = $event.target.value"
              /><br />
              <label for="name">Nombre</label><br />
              <input type="text" @change="name = $event.target.value" /><br />
              <label for="photoUrls">Url de Fotos</label><br />
              <input
                type="text"
                @change="photoUrls = $event.target.value"
              /><br />
              <label for="tags">Tags</label><br />
              <input type="text" @change="tags = $event.target.value" /><br />
              <label for="status">Estatus</label><br />
              <input type="text" @change="status = $event.target.value" /><br />
              <button type="submit" @click="created">Guardar</button>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="active" class="modal-backdrop fade show"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      data: {},
      active: false,
      show: false
    };
  },
  beforeMount() {
    this.getName();
  },
  methods: {
    toggleModal() {
      const body = document.querySelector("body");
      this.active = !this.active;
      this.active
        ? body.classList.add("modal-open")
        : body.classList.remove("modal-open");
      setTimeout(() => (this.show = !this.show), 10);
    },
    async getName() {
      const res = await fetch("http://localhost:8000/api/pets");
      const data = await res.json();
      this.data = data;
      console.log(data);
    },
    created() {
      // Simple POST request with a JSON body using fetch
      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          category: this.category,
          name: this.name,
          photoUrls: [this.photoUrls],
          tags: [this.tags],
          status: this.status
        })
      };
      fetch("http://localhost:8000/api/pet", requestOptions)
        .then(response => response.json())
        .then(data => (this.postId = data.id));
    }
  }
};
</script>
