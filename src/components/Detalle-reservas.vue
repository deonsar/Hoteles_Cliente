<template>

	<div style="min-width:450px; text-align: left;  display:inline-block; vertical-align:top">
		<div>
		  <h2 style="overflow: hidden; text-align:center"><strong> Gestion Reserva </strong></h2>

		  <br>

		  <label class="w3-text" for="hoteles" style="font-size: 20px;background: white"> <strong>Elegir Hotel </strong></label>
		  <select class="w3-select w3-border" name="hoteles" style="overflow: hidden; text-overflow: ellipsis" value="Hoteles" :disabled="!editing && !addingNew" v-model="Reserva.Hoteles">
		    <option v-for="item in HotelesSelect" :value="item.Nombre"> Hotel: {{item.Nombre}}  
		     -- Direccion {{item.Direccion}}</option>
		  </select>

		  <br><br>

		  <ul class="ul-Usuario">
		  	<li class="usuario">

			  <label class="w3-text " for="nombre"> Nombre: </label>
			  <br>
			  <input class=" w3-border campos" style="width:95%; background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Nombre">

		  	</li>

		  	<li class="usuario">
			  <label class="w3-text" for="apellido"> Apellidos: </label>
			  <br>
			  <input class="w3-border campos" style="width:95%; background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Apellido">
		  	</li>

		  </ul>

		  <ul class="ul-Usuario">

		  	<li class="usuario" >
			  <label class="w3-text" for="email"> Email </label>
			  <br>
			  <input class="w3-input w3-border" style="width:95%; background: white; overflow: hidden; text-overflow: ellipsis" :disabled="!editing && !addingNew" v-model="Reserva.Email">
		  	</li>

		  	<li class="usuario">
			  <label class="w3-text" for="telefono"> Telefono </label>
			  <br>
			  <input class="w3-input w3-border" style="width:87%;background: white; overflow: hidden; text-overflow: ellipsis" type="number" 
			  :disabled="!editing && !addingNew" v-model="Reserva.Telefono">
		  	</li>

		  	
		  </ul>
		  
		  <ul class="ul-Usuario">
		  
			<li class="usuario">

			  <label class="w3-text" for="fechaSalida"> Fecha Salida </label>
			  <br>
		      <input class="w3-input w3-border" style="width:95%; background: white; overflow: hidden; text-overflow: ellipsis" type="date" :disabled="!editing && !addingNew" v-model="Reserva.FechaSalida">		  
		  
		  	</li>

		  	<li class="usuario">
			  <label class="w3-text" for="fecha"> Fecha Llegada </label>
			  <input class="w3-input w3-border" style="width:95%; background: white; overflow: hidden; text-overflow: ellipsis" type="date" :disabled="!editing && !addingNew" v-model="Reserva.FechaLlegada">
		  	</li>
		  </ul>		  

		  <br>


		  <ul class="ul-personas">

		  <li class="personas">
			  <label class="w3-text" for="habitaciones" style="background: white"> Habitaciones </label>
			  <select class="w3-select w3-border" name="habitaciones" style="overflow: hidden; text-overflow: ellipsis" value="Habitaciones" :disabled="!editing && !addingNew" v-model="Reserva.Habitaciones">
			    <option value="1" selected="selected">1</option>
			    <option value="2">2</option>
			    <option value="3">3</option>
			    <option value="4">4</option>
			  </select>
		  </li>

		  <li class="personas">
			  <label class="w3-text" for="adultos" style="background: white"> Adultos </label>
			  <select class="w3-select w3-border" name="adultos" style="overflow: hidden; text-overflow: ellipsis" value="Adultos" :disabled="!editing && !addingNew" v-model="Reserva.Adultos">			
			    <option value="1" selected="selected">1</option>
			    <option value="2">2</option>
			    <option value="3">3</option>
			    <option value="4">4</option>
			    <option value="5">5</option>
			    <option value="6">6</option>
			  </select>
		  </li>

		  <li class="personas">

			  <label class="w3-text" for="ninos" style="background: white"> Niños </label>
			  <select class="w3-select w3-border" name="ninos" style="overflow: hidden; text-overflow: ellipsis" value="Ninos" :disabled="!editing && !addingNew" v-model="Reserva.Ninos">
			    <option value="0" selected="selected">0</option>
			    <option value="1">1</option>
			    <option value="2">2</option>
			    <option value="3">3</option>
			    <option value="4">4</option>
			  </select>

		  </li>

		  </ul>		
		  <br>	  
		</div>

		<br>

		<div>
			<template v-if="!addingNew">
			  <div style="float: left">
			    <button type="button" class="btn btn-default btn-sm" title="Nuevo" @click="editNew" :disabled="editing">
			      <app-icon img="plus"> </app-icon> Nueva Reserva 
			    </button>
			  </div>
			</template>
			<template v-else>
			  <div style="float: left">
			    <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateNew">
			      <app-icon img="ok" style="color: green"></app-icon> Aceptar
			    </button>
			    <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discardNew">
			      <app-icon img="remove" style="color: red"></app-icon> Cancelar
			    </button>
			  </div>
			</template>

			<template v-if="!editing">
		        <div style="float: right">
		          <button type="button" class="btn btn-default btn-sm" title="Editar" @click="validateIdUpdate" :disabled="addingNew">
		            <app-icon img="edit" ></app-icon> Editar Reserva
		          </button>
		          <button type="button" class="btn btn-default btn-sm" title="Eliminar" @click="validateIdDelete" :disabled="addingNew">
		            <app-icon img="trash"></app-icon> Eliminar Reserva
		          </button>
		        </div>
      		</template>
			<template v-else>
			  <div style="float: right">
			    <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateUpdate">
			      <app-icon img="ok" style="color: green"></app-icon> Aceptar
			    </button>
			    <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discard">
			      <app-icon img="remove" style="color: red"></app-icon> Cancelar
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
	  	    mensaje = 'Selecciona una reserva de la lista.';
	  	    EventBus.$emit('showMessage', mensaje);
	  	  } else {
	  	    this.edit();
	  	  }
	  	},
	  	validateIdDelete: function() {
	  	  let mensaje ='';
	  	  if(this.Reserva.Id =='' || this.Reserva.Id < 0) {
	  	    mensaje = 'Selecciona una reserva de la lista.';
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

            }
          }
        },
        mounted: function() {
          EventBus.$on('reservaSelected', function(id) {
            this.load(id);
            this.editing = false;
            this.addingNew = false;
          }.bind(this));

          EventBus.$on('objHoteles', function(obj) {
            this.HotelesSelect = obj;
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

.campos{
	padding:8px;
	display: block;

}

.ul-Usuario{
	list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;    
}

.usuario{
   float : left;
  
   width: 50%;
}
.ul-personas{
	list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;    
}

.personas{
   float : left;
   padding: 10px;
   width: 33%;
}

select{
	border-radius: 10px;
}
</style>