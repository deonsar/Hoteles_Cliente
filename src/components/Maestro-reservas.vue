<template>
  <div style="min-width:40%; display:inline-block; vertical-align:top">
    
    <ul class="w3-ul w3-card-4">
      <li><h2><strong>Reservas</strong></h2></li>   
      
      <li style="text-align: left; overflow: hidden; text-overflow: ellipsis" class="w3-hover-blue" v-for="Reserva in Reservas" @click="reservaSelected(Reserva.Id, mostrar)"> 
        
        <div style="font-size: 20px;">
        <span class="glyphicon glyphicon-user" ></span>
        &nbsp;
        {{Reserva.Nombre}} {{Reserva.Apellido}}

         <div style="float: right; font-size: 10px;">
            Salida: {{Reserva.FechaSalida}} - Llegada: {{Reserva.FechaLlegada}}
          </div>
        </div>        

        <div style="float: right;">
        <span class="glyphicon glyphicon-earphone"></span>
        {{Reserva.Telefono}}
        </div>

        <div>
        <span class="glyphicon glyphicon-home"></span>
        {{Reserva.Hoteles}}
        </div>
        </li>
    </ul>
  </div>
</template>

<script>

import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'
import appConfig from './config.js'

var httpURL = appConfig.URLReservas;

export default {
  components: {
    'app-icon' : AppIcon
  },

  

  methods: {
  	loadList: function(){
      var _this = this;
      $.ajax(
        {
          url : httpURL,
          type: "GET",
        })
        .done(function(data) {
          _this.Reservas = data;
        })
        .fail(function(data) {
          let mensaje = 'No se pudo cargar la lista. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
    },

    reservaSelected: function(id, mostrar){
    	EventBus.$emit('reservaSelected', id);

      if(this.mostrar===true){        
        EventBus.$emit('showReservaDet', mostrar);
        this.mostrar = false;
      }
      else{
        EventBus.$emit('showReservaDet', mostrar);
        this.mostrar = true;
      }
    }

  },

  data: function () {
    return {
      Reservas: [],
      mostrar: true
    }
  },

  mounted: function() {
    this.loadList();

	  EventBus.$on('updateListReserva', function() {
	    this.loadList();
	  }.bind(this));
  }

}

</script>