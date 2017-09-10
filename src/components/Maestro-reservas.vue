<template>
  <div style="min-width:300px; max-width:300px; display:inline-block; vertical-align:top">
    <ul class="w3-ul w3-card-4">
      <li><h2><strong>Reservas</strong></h2></li>
      <li style="overflow: hidden; text-overflow: ellipsis" class="w3-hover-blue" v-for="Reserva in Reservas" @click="reservaSelected(Reserva.Id)"> 
        <span class="glyphicon glyphicon-eye-open"></span>
        &nbsp;
        {{Reserva.Hotel}}</li>
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

  data: function () {
    return {
      Reservas: [],
    }
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

    reservaSelected: function(id){
    	EventBus.$emit('reservaSelected', id);
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