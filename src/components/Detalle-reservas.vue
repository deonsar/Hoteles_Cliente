<template>
	<div  class="w3-container w3-card-4" style="min-width:500px; text-align: left; max-width:300px; display:inline-block; vertical-align:top">
		<div>
		  <h3 style="overflow: hidden; max-width:400px"><strong> Gestion Reserva </strong></h3>

		  <label class="w3-text" for="nombre"> Nombre: </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Nombre">
		  <br>

		  <label class="w3-text" for="apellido"> Apellidos: </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Apellido">
		  <br>

		  <label class="w3-text" for="telefono"> Telefono </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="number" 
		  :disabled="!editing && !addingNew" v-model="Reserva.Telefono">
		  <br>

		  <label class="w3-text" for="email"> Email </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Email">
		  <br>
		  
		  <label class="w3-text" for="fechaSalida"> FechaSalida </label>
	      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="date" :disabled="!editing && !addingNew" v-model="Reserva.FechaSalida">		  
		  <br>

		  <label class="w3-text" for="fecha"> FechaLlegada </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="date" :disabled="!editing && !addingNew" v-model="Reserva.FechaLlegada">
		  <br>

		  <label class="w3-text" for="hoteles" style="background: white"> Hoteles </label>
		  <select class="w3-select w3-border" name="hoteles" style="overflow: hidden; text-overflow: ellipsis" value="Hoteles" :disabled="!editing && !addingNew" v-model="Reserva.Hoteles">
		    <option value="alta" selected="selected" >Alta</option>
		    <option value="media">Media</option>
		    <option value="Baja">Baja</option>
		  </select>

		  <label class="w3-text" for="habitaciones" style="background: white"> Habitaciones </label>
		  <select class="w3-select w3-border" name="habitaciones" style="overflow: hidden; text-overflow: ellipsis" value="Habitaciones" :disabled="!editing && !addingNew" v-model="Reserva.Habitaciones">
		    <option value="1" selected="selected">1</option>
		    <option value="2">2</option>
		    <option value="3">3</option>
		    <option value="4">4</option>
		  </select>

		  <label class="w3-text" for="adultos" style="background: white"> Adultos </label>
		  <select class="w3-select w3-border" name="adultos" style="overflow: hidden; text-overflow: ellipsis" value="Adultos" :disabled="!editing && !addingNew" v-model="Reserva.Adultos">
		  	<option value="0" selected="selected">0</option>
		    <option value="1">1</option>
		    <option value="2">2</option>
		    <option value="3">3</option>
		    <option value="4">4</option>
		    <option value="5">5</option>
		    <option value="6">4</option>
		  </select>

		  <label class="w3-text" for="ninos" style="background: white"> Niños </label>
		  <select class="w3-select w3-border" name="ninos" style="overflow: hidden; text-overflow: ellipsis" value="Ninos" :disabled="!editing && !addingNew" v-model="Reserva.Ninos">
		    <option value="0" selected="selected">0</option>
		    <option value="1">1</option>
		    <option value="2">2</option>
		    <option value="3">3</option>
		    <option value="4">4</option>
		  </select>

		  <label class="w3-text" for="PrecioTotal"> Precio Total </label>
		  <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.PrecioTotal">
		  <br>
		  
		</div>

		<br>

		<div>
			<template v-if="!addingNew">
			  <div style="float: left">
			    <button type="button" class="btn btn-default btn-sm" title="Nuevo" @click="editNew" :disabled="editing">
			      <app-icon img="plus"></app-icon>
			    </button>
			  </div>
			</template>
			<template v-else>
			  <div style="float: left">
			    <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateNew">
			      <app-icon img="ok"></app-icon>
			    </button>
			    <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discardNew">
			      <app-icon img="remove"></app-icon>
			    </button>
			  </div>
			</template>

			<template v-if="!editing">
		        <div style="float: right">
		          <button type="button" class="btn btn-default btn-sm" title="Editar" @click="validateIdUpdate" :disabled="addingNew">
		            <app-icon img="edit"></app-icon>
		          </button>
		          <button type="button" class="btn btn-default btn-sm" title="Eliminar" @click="validateIdDelete" :disabled="addingNew">
		            <app-icon img="trash"></app-icon>
		          </button>
		        </div>
      		</template>
			<template v-else>
			  <div style="float: right">
			    <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateUpdate">
			      <app-icon img="ok"></app-icon>
			    </button>
			    <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discard">
			      <app-icon img="remove"></app-icon>
			    </button>
			  </div>
			</template>
		</div>

		<div>
		  <p style="visibility: hidden">separador</p>
		</div>

		<br>
	</div>
</template>

<script>
	import EventBus from './event-bus.js'
	import AppIcon from './App-icon.vue'
	import appConfig from './config.js'
	import InfoMessage from './InfoMessage.vue'

	var httpURL = appConfig.URLReservas;
	var maxInt =  2147483647;

	export default {
	  components: {
	    'app-icon' : AppIcon,
	    'infomessage' : InfoMessage
	  },	

	  methods:{
	  	validateNew: function() {
	  	  let mensaje ='';
	  	  var ValidarEmail = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

	  	  if(this.Reserva.Nombre == '') {
	  	    mensaje = 'El campo "Nombre" no puede estar vacío.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.Reserva.Apellido == '') {
	  	    mensaje = 'El campo "Apellido" no puede estar vacío.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(!this.isInt(this.Reserva.Telefono) || this.Reserva.Telefono == '' ||  this.Reserva.Telefono.length !== 9) {
	        mensaje = 'Error en el campo "Telefono". Comprobar que el telefono contenga 9 digitos';
	        EventBus.$emit('showMessage', mensaje);

	  	  } else if(!ValidarEmail.test(this.Reserva.Email)|| this.Reserva.Email == '') {
	  	    mensaje = 'Error en el campo "Email". Comprobar que los datos son correctos';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.isNumeric(this.Reserva.FechaSalida) || this.Reserva.FechaSalida == '') {
	  	    mensaje = 'La fecha de salida no puede estar vacío, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.isNumeric(this.Reserva.FechaLlegada) || this.Reserva.FechaLlegada == '') {
	  	    mensaje = 'La fecha de llegada no puede estar vacío, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.Reserva.FechaSalida > this.Reserva.FechaLlegada) {
	  	    mensaje = 'La fecha de salida no puede ser mayor que la fecha de llegada.';
	  	    EventBus.$emit('showMessage', mensaje); 
	  	  
	  	  } else if(this.isNumeric(this.Reserva.Fecha) || this.Reserva.FechaSalida == '') {
	  	    mensaje = 'La Fecha de la tarea no puede estar vacía, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else {
	  	    this.create();
	  	  }
	  	},
	  	validateIdUpdate: function() {
	  	  let mensaje ='';
	  	  if(this.Reserva.Id =='' || this.Reserva.Id < 0) {
	  	    mensaje = 'Seleccione un perfil de la lista.';
	  	    EventBus.$emit('showMessage', mensaje);
	  	  } else {
	  	    this.edit();
	  	  }
	  	},
	  	validateIdDelete: function() {
	  	  let mensaje ='';
	  	  if(this.Reserva.Id =='' || this.Reserva.Id < 0) {
	  	    mensaje = 'Seleccione una película de la lista.';
	  	    EventBus.$emit('showMessage', mensaje);
	  	  } else {
	  	    this.remove();
	  	  }
	  	},
	  	validateUpdate: function() {
	  	  let mensaje ='';
	  	  var ValidarEmail = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

	  	  if(this.Reserva.Nombre == '') {
	  	    mensaje = 'El campo "Nombre" no puede estar vacío.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.Reserva.Apellido == '') {
	  	    mensaje = 'El campo "Apellido" no puede estar vacío.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(!this.isInt(this.Reserva.Telefono) || this.Reserva.Telefono == '' ||  this.Reserva.Telefono.length !== 9) {
	        mensaje = 'Error en el campo "Telefono". Comprobar que el telefono contenga 9 digitos';
	        EventBus.$emit('showMessage', mensaje);

	  	  } else if(!ValidarEmail.test(this.Reserva.Email)|| this.Reserva.Email == '') {
	  	    mensaje = 'Error en el campo "Email". Comprobar que los datos son correctos';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.isNumeric(this.Reserva.FechaSalida) || this.Reserva.FechaSalida == '') {
	  	    mensaje = 'La fecha de salida no puede estar vacío, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.isNumeric(this.Reserva.FechaLlegada) || this.Reserva.FechaLlegada == '') {
	  	    mensaje = 'La fecha de llegada no puede estar vacío, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);

	  	  } else if(this.Reserva.FechaSalida > this.Reserva.FechaLlegada) {
	  	    mensaje = 'La fecha de salida no puede ser mayor que la fecha de llegada.';
	  	    EventBus.$emit('showMessage', mensaje); 
	  	  
	  	  } else if(this.isNumeric(this.Reserva.Fecha) || this.Reserva.FechaSalida == '') {
	  	    mensaje = 'La Fecha de la tarea no puede estar vacía, seleccione una fecha válida.';
	  	    EventBus.$emit('showMessage', mensaje);
	  	  } else {
  	  	    this.update();
  	  	  }
	  	},
	  	isNumeric: function(n) {
	  	  return !isNaN(parseFloat(n)) && isFinite(n);
	  	},
	  	isInt: function(n) {
	  	  return n % 1 === 0;
	  	},
	  	editNew: function () {
	  	  this.ReservaCopia.Nombre = this.Reserva.Nombre;
	  	  this.ReservaCopia.Apellido = this.Reserva.Apellido;
	  	  this.ReservaCopia.Telefono = this.Reserva.Telefono;
	  	  this.ReservaCopia.Email = this.Reserva.Email;
	  	  this.ReservaCopia.FechaSalida= this.Reserva.FechaSalida;
	  	  this.ReservaCopia.FechaLlegada= this.Reserva.FechaLlegada;
	  	  this.ReservaCopia.Hoteles= this.Reserva.Hoteles;
	  	  this.ReservaCopia.Habitaciones= this.Reserva.Habitaciones;
	  	  this.ReservaCopia.Adultos= this.Reserva.Adultos;
	  	  this.ReservaCopia.Ninos= this.Reserva.Ninos;
	  	  this.ReservaCopia.PrecioTotal= this.Reserva.PrecioTotal;

	  	  this.Reserva.Nombre = '';
	  	  this.Reserva.Apellido = '';
	  	  this.Reserva.Telefono = '';
	  	  this.Reserva.Email = '';
	  	  this.Reserva.FechaSalida = '';
	  	  this.Reserva.FechaLlegada = '';
	  	  this.Reserva.Hoteles = '';
	  	  this.Reserva.Habitaciones = '';
	  	  this.Reserva.Adultos = '';
	  	  this.Reserva.Ninos = '';
	  	  this.Reserva.PrecioTotal = '';
	  	  this.addingNew = true;
	  	},
	  	discardNew: function () {
	  	  this.Reserva.Nombre = this.ReservaCopia.Nombre;
	  	  this.Reserva.Apellido = this.ReservaCopia.Apellido;
	  	  this.Reserva.Telefono = this.ReservaCopia.Telefono;
	  	  this.Reserva.Email = this.ReservaCopia.Email;
	  	  this.Reserva.FechaSalida= this.ReservaCopia.FechaSalida;
	  	  this.Reserva.FechaLlegada= this.ReservaCopia.FechaLlegada;
	  	  this.Reserva.Hoteles= this.ReservaCopia.Hoteles;
	  	  this.Reserva.Habitaciones= this.ReservaCopia.Habitaciones;
	  	  this.Reserva.Adultos= this.ReservaCopia.Adultos;
	  	  this.Reserva.Ninos= this.ReservaCopia.Ninos;
	  	  this.Reserva.PrecioTotal= this.ReservaCopia.PrecioTotal;
	  	  this.addingNew = false;
	  	},
	  	edit: function () {
	  	  this.ReservaCopia.Nombre = this.Reserva.Nombre;
	  	  this.ReservaCopia.Apellido = this.Reserva.Apellido;
	  	  this.ReservaCopia.Telefono = this.Reserva.Telefono;
	  	  this.ReservaCopia.Email = this.Reserva.Email;
	  	  this.ReservaCopia.FechaSalida= this.Reserva.FechaSalida;
	  	  this.ReservaCopia.FechaLlegada= this.Reserva.FechaLlegada;
	  	  this.ReservaCopia.Hoteles= this.Reserva.Hoteles;
	  	  this.ReservaCopia.Habitaciones= this.Reserva.Habitaciones;
	  	  this.ReservaCopia.Adultos= this.Reserva.Adultos;
	  	  this.ReservaCopia.Ninos= this.Reserva.Ninos;
	  	  this.ReservaCopia.PrecioTotal= this.Reserva.PrecioTotal;
	  	  this.editing = true;
	  	},
	  	discard: function () {
	  	  this.Reserva.Nombre = this.ReservaCopia.Nombre;
	  	  this.Reserva.Apellido = this.ReservaCopia.Apellido;
	  	  this.Reserva.Telefono = this.ReservaCopia.Telefono;
	  	  this.Reserva.Email = this.ReservaCopia.Email;
	  	  this.Reserva.FechaSalida= this.ReservaCopia.FechaSalida;
	  	  this.Reserva.FechaLlegada= this.ReservaCopia.FechaLlegada;
	  	  this.Reserva.Hoteles= this.ReservaCopia.Hoteles;
	  	  this.Reserva.Habitaciones= this.ReservaCopia.Habitaciones;
	  	  this.Reserva.Adultos= this.ReservaCopia.Adultos;
	  	  this.Reserva.Ninos= this.ReservaCopia.Ninos;
	  	  this.Reserva.PrecioTotal= this.ReservaCopia.PrecioTotal;
	  	  this.editing = false;
	  	},
	  	cleanForm: function() {	  	  
	  	  this.Reserva.Nombre = '';
	  	  this.Reserva.Apellido = '';
	  	  this.Reserva.Telefono = '';
	  	  this.Reserva.Email = '';
	  	  this.Reserva.FechaSalida = '';
	  	  this.Reserva.FechaLlegada = '';
	  	  this.Reserva.Hoteles = '';
	  	  this.Reserva.Habitaciones = '';
	  	  this.Reserva.Adultos = '';
	  	  this.Reserva.Ninos = '';
	  	  this.Reserva.PrecioTotal = '';
	  	  this.Reserva.Id = '';
	  	},

	  	create: function () {
	  	  var _this = this;
	  	  $.ajax(
	  	    {
	  	      url : httpURL,
	  	      type: "POST",
	  	      data: {
	  	        Nombre: this.Reserva.Nombre,
	  	        Apellido: this.Reserva.Apellido,
	  	        Telefono: this.Reserva.Telefono,
	  	        Email: this.Reserva.Email,
	  	        FechaSalida: this.Reserva.FechaSalida,
	  	        FechaLlegada: this.Reserva.FechaLlegada,
	  	        Hoteles: this.Reserva.Hoteles,
	  	        Habitaciones: this.Reserva.Habitaciones,
	  	        Adultos: this.Reserva.Adultos,
	  	        Ninos: this.Reserva.Ninos,
	  	        PrecioTotal: this.Reserva.PrecioTotal
	  	      }

	  	    })
	  	    .done(function(data) {
	  	      EventBus.$emit('updateListReserva');
	  	      let mensaje ='Reserva creado con éxito.';
	  	      EventBus.$emit('showMessage', mensaje);
	  	      _this.addingNew = false;
	  	    })
	  	    .fail(function(data) {
	  	      let mensaje = 'No se puede crear la reserva. Revise su conexión a Internet.';
	  	      EventBus.$emit('showMessage', mensaje);
	  	    });
	  	},
	  	update: function () {
	  	  var _this = this;
	  	  $.ajax(
	  	    {
	  	      url : httpURL + this.Reserva.Id,
	  	      type: "PUT",
	  	      data: {
	  	        Id: this.Reserva.Id,
	  	        Nombre: this.Reserva.Nombre,
	  	        Apellido: this.Reserva.Apellido,
	  	        Telefono: this.Reserva.Telefono,
	  	        Email: this.Reserva.Email,
	  	        FechaSalida: this.Reserva.FechaSalida,
	  	        FechaLlegada: this.Reserva.FechaLlegada,
	  	        Hoteles: this.Reserva.Hoteles,
	  	        Habitaciones: this.Reserva.Habitaciones,
	  	        Adultos: this.Reserva.Adultos,
	  	        Ninos: this.Reserva.Ninos,
	  	        PrecioTotal: this.Reserva.PrecioTotal
	  	      }
	  	    })
	  	    .done(function(data) {
	  	      EventBus.$emit('updateListReserva');
	  	      let mensaje ='Reserva actualizado con éxito.';
	  	      EventBus.$emit('showMessage', mensaje);
	  	      _this.cleanForm();
	  	      _this.editing = false;
	  	    })
	  	    .fail(function(data) {
	  	      let mensaje = 'No se puede actualizar la reserva. Revise su conexión a Internet.';
	  	      EventBus.$emit('showMessage', mensaje);
	  	    });
	  	},
	  	remove: function () {
          var _this = this;
          $.ajax(
            {
              url : httpURL + this.Reserva.Id,
              type: "DELETE",
              data: {Id: this.Reserva.Id}
            })
            .done(function(data) {
              EventBus.$emit('updateListReserva');
              _this.cleanForm();
              let mensaje ='Reserva eliminado con éxito.';
              EventBus.$emit('showMessage', mensaje);
              _this.editing = false;
            })
            .fail(function(data) {
              let mensaje = 'No se puede eliminar la reserva. Revise su conexión a Internet.';
              EventBus.$emit('showMessage', mensaje);
            });
        },
        load: function(id){
          var _this = this;
          $.ajax(
            {
              url : httpURL + id,
              type: "GET",
            })
            .done(function(data) {
              _this.Reserva = data;
            })
            .fail(function(data) {
              let mensaje = 'No se pudo cargar la reserva. Revise su conexión a Internet.';
              EventBus.$emit('showMessage', mensaje);
            });
          },
        },
        data: function () {
          return {
            editing: false,
            addingNew: false,
            HotelesSelect: {},
            Reserva: {
              Id: '',
              Nombre: '',
              Apellido: '',
              Telefono: '',
              Email: '',
              FechaSalida:'',
              FechaLlegada:'',
              Hoteles:'',
              Habitaciones:'',
              Adultos:'',
              Ninos:'',
              PrecioTotal:''
            },
            ReservaCopia: {
              Id: '',
              Nombre: '',
              Apellido: '',
              Telefono: '',
              Email: '',
              FechaSalida:'',
              FechaLlegada:'',
              Hoteles:'',
              Habitaciones:'',
              Adultos:'',
              Ninos:'',
              PrecioTotal:''
            }
          }
        },
        mounted: function() {
          EventBus.$on('reservaSelected', function(id) {
            this.load(id);
            this.editing = false;
            this.addingNew = false;
          }.bind(this));
        }
	  
	}

</script>

<style lang="scss">

input[type="radio"] {
  display: none;
}



p {
  text-align: center;
}


.clasificacion {
  direction: rtl;
  unicode-bidi: bidi-override;
}

.star{
  font-size: 30px;
  color: grey;

}


:hover ~ .star {
  color: orange;
}

input[type="radio"]:checked ~ label {
  color: orange;
}
</style>