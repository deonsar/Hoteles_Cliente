<template>
  <div style="min-width:500px; display:inline-block; vertical-align:top">

    <ul class="w3-ul w3-card-4">
      <li ><h2><strong>Hoteles</strong></h2> </li>     
     <!--  <li style="padding: none;">
        <button class="btn btn-default btn-lg" style="border : none; width:100%">Agregar Hotel</button>
      </li> -->
      <li style="text-align: left;overflow: hidden; text-overflow: ellipsis" class="w3-hover-blue" v-for="Hotel in Hoteles" @click="hotelSelected(Hotel.Id, mostrar)"> 
        <div style="font-size: 20px;">
        <span class="glyphicon glyphicon-search" ></span>
        &nbsp;
        {{Hotel.Nombre}} 

        <div style="float: right; font-size: 18px;">
           {{Hotel.Puntuacion}} <span style="color: orange;">★</span>
          </div>
        </div>

        

        <div style="float: left;">
        <span class="glyphicon glyphicon-map-marker"></span>
        {{Hotel.Direccion}}
        </div>

        <div style="float: right;">
        
        Precio Desde: {{Hotel.PrecioBase}}
        <span class="glyphicon glyphicon-euro"></span>
        </div>


        

      </li>
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