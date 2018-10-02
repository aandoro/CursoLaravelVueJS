<template>
	<div>
		<div class="alert alert-success" v-if="saved">
			<strong>Success!</strong> Your vianda has been saved successfully.
		</div>

		<div class="well well-sm" id="vianda-form">
			<form class="form-horizontal" method="post" @submit.prevent="onSubmit">
				<div class="form-group">
					<label>Nombre</label>
					<input type="text" v-model="vianda.nombre" class="form-control input-sm" placeholder="Nombre de la viandita">
				</div>
				<div class="form-group">
					<label>Descripción</label>
					<textarea v-model="vianda.descripcion" class="form-control input-sm" placeholder="Descripción de la viandita"></textarea>
				</div>
        <div class="row">
          <div class="col-xs-6 col-sm-6 col-md-6">
            <div class="form-group">
              <label>Precio</label>
              <input type="number" v-model="vianda.precio" class="form-control input-sm">
            </div>
          </div>
          <div class="col-xs-6 col-sm-6 col-md-6">
            <div class="form-check">
              <input class="form-check-input" type="checkbox" v-model="vianda.vegetariana">
              <label class="form-check-label">
                Vegetariana
              </label>
            </div>
          </div>
        </div>
				<button type="submit" class="btn btn-primary">Guardar</button>
			</form>
		</div>
	</div>
</template>

<script>
export default {
  data() {
    return {
      errors: [],
      saved: false,
      vianda: {
        nombre: null,
        descripcion: null,
        precio: 0,
        vegetariana: false
      },
      endpoint: "api/vianda/"
    };
  },
  created() {
    this.fetch();
  },

  methods: {
    fetch(page = 1) {
      axios.get(this.endpoint + id).then(({ data }) => {
        this.vianda = data.data;
        this.pageCount = data.meta.last_page;
      });
    },
    onSubmit() {
      this.saved = false;

      axios
        .post("api/vianda", this.vianda)
        .then(({ data }) => this.setSuccessMessage())
        .catch(({ response }) => this.setErrors(response));
    },

    setErrors(response) {
      this.errors = response.data.errors;
    },

    setSuccessMessage() {
      this.reset();
      this.saved = true;
    },

    reset() {
      this.errors = [];
      this.vianda = {
        nombre: null,
        descripcion: null,
        precio: 0,
        vegetariana: false
      };
    }
  }
};
</script>
