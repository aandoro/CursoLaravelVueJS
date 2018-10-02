<template>
	<div>
		<div class="alert alert-success" v-if="saved">
			<strong>Success!</strong> Your pedido has been saved successfully.
		</div>
		<div class="well well-sm" id="pedido-form">
			<form class="form-horizontal" method="post" @submit.prevent="onSubmit">
				<div class="form-group">
					<label>Cliente</label>
          <b-form-select v-model="pedido.persona_id" :options="arrayDePersonas" class="mb-3" value-field="id" text-field="nombre" />
				</div>
				<div class="form-group">
					<label>Vianda solicitada</label>
          <b-form-select v-model="pedido.vianda_id" :options="arrayDeViandas" class="mb-3" value-field="id" text-field="nombre" />
				</div>
				<div class="row">
								<div class="col-xs-6 col-sm-6 col-md-6">
									<div class="form-group">
                    <label>Fecha de solicitud</label>
										<input type="date" v-model="pedido.fecha_solicitud" class="form-control input-sm" placeholder="dd-mm-aaaa">
									</div>
								</div>
								<div class="col-xs-6 col-sm-6 col-md-6">
									<div class="form-group">
                    <label>Fecha de entrega</label>
										<input type="date" v-model="pedido.fecha_entrega" class="form-control input-sm" placeholder="dd-mm-aaaa">
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
      pedido: {
        persona_id: null,
        vianda_id: null,
        fecha_solicitud: null,
        fecha_entrega: null
      },
      arrayDePersonas: [],
      arrayDeViandas: [],
      endpointp: "api/personas?page=",
      endpointv: "api/viandas?page="
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch(page = 1) {
      axios.get(this.endpointp + page).then(({ data }) => {
        this.arrayDePersonas = data.data;
      });
      axios.get(this.endpointv + page).then(({ data }) => {
        this.arrayDeViandas = data.data;
      });
    },
    onSubmit() {
      this.saved = false;

      axios
        .post("api/pedidos", this.pedido)
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
      this.pedido = {
        persona_id: null,
        vianda_id: null,
        fecha_solicitud: null,
        fecha_entrega: null
      };
      arrayDePersonas = [];
      arrayDeViandas = [];
    }
  }
};
</script>
