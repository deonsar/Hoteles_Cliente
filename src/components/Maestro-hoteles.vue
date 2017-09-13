<template>
  <div style="min-width:300px; max-width:300px; display:inline-block; vertical-align:top">
    <ul class="w3-ul w3-card-4">
      <li><h2><strong>Hoteles</strong></h2></li>     
      <li style="overflow: hidden; text-overflow: ellipsis" class="w3-hover-blue" v-for="Hotel in Hoteles" @click="hotelSelected(Hotel.Id, mostrar)"> 
        <span class="glyphicon glyphicon-eye-open"></span>
        &nbsp;
        {{Hotel.Nombre}}</li>
    </ul>
  </div>
</template>

<script>
import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'
import appConfig from './config.js'

var httpURL = appConfig.URLHoteles;

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
          _this.Hoteles = data;
           EventBus.$emit('objHoteles', data);
        })
        .fail(function(data) {
          let mensaje = 'No se pudo cargar la lista. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
    },

    hotelSelected: function(id, mostrar){
      EventBus.$emit('hotelSelected', id);
      
      if(this.mostrar===true){        
        EventBus.$emit('showHotelDet', mostrar);
        this.mostrar = false;
      }
      else{
        EventBus.$emit('showHotelDet', mostrar);
        this.mostrar = true;
      }

    }
  },

    data: function () {
      return {
        Hoteles: [],
        mostrar: true
      }
    },

    mounted: function() {
      this.loadList();

      EventBus.$on('updateListHotel', function() {
        this.loadList();
      }.bind(this));
    },
   
  }

  </script>