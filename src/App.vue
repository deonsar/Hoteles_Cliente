<template>
  <div id="app">
    <master-chooser></master-chooser>
    <h1>{{msg}}</h1> 

    <div v-show="opcion == 'hoteles'">
      
      <h2><strong>{{ title.Hoteles }}</strong></h2>
      <maestro-hoteles ></maestro-hoteles>    
      
      <div  id="myModal" class="modal w3-container" style="display:inline-block;" v-show="showHotelesDet">
        <div class="modal-content  w3-animate-top ">
          
          <span class="close" @click="showHotelesDet = false">&times;</span>
          <detalle-hoteles></detalle-hoteles>

        </div>
      </div>

    </div>

    <div v-show="opcion == 'reservas'">
      <h2><strong>{{ title.Reservas }}</strong></h2>
      <maestro-reservas ></maestro-reservas>
      <detalle-reservas v-show="showReservasDet" ></detalle-reservas>
    </div>
    
    <infomessage style="clear:both"></infomessage>
  </div>
</template>

<script>
import EventBus from './components/event-bus.js'
import Chooser from './components/Chooser.vue'
import MaestroHoteles from './components/Maestro-hoteles.vue'
import DetalleHoteles from './components/Detalle-hoteles.vue'
import MaestroReservas from './components/Maestro-reservas.vue'
import DetalleReservas from './components/Detalle-reservas.vue'
import InfoMessage from './components/InfoMessage.vue'


export default {
  name: 'app',

  components: {
    'master-chooser' : Chooser,
    'maestro-hoteles' : MaestroHoteles,
    'detalle-hoteles': DetalleHoteles,    
    'maestro-reservas' : MaestroReservas,
    'detalle-reservas' : DetalleReservas,
    'infomessage' : InfoMessage
  },

  data () {
    return {
      title:{
        Hoteles:"Hoteles",
        Reservas:"Reservas"

      },

      msg: 'Welcome to Your Vue.js App',
      opcion:"hoteles",
      //Datos para mostrar los detalles
      showHotelesDet: false,
      showReservasDet: false
    }
  },
  
  mounted: function(){
    EventBus.$on('chooseOption', function(opcion) {
      this.opcion = opcion;
    }.bind(this));
    EventBus.$on('showHotelDet', function(mostrar) {
      this.showHotelesDet = mostrar;
    }.bind(this));

    EventBus.$on('showReservaDet', function(mostrar) {
      this.showReservasDet = mostrar;
    }.bind(this));
  }
}
</script>

<style >
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}



a {
  color: #42b983;
}

.modal {
    display: none; /* Hidden by default */
    position: fixed; /* Stay in place */
    z-index: 1; /* Sit on top */
    padding-top: 100px; /* Location of the box */
    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */
    overflow: auto; /* Enable scroll if needed */
    background-color: rgb(0,0,0); /* Fallback color */
    background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
    
    background-color: #fefefe;
    margin: auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;

    
}

/* The Close Button */
.close {
    color: #aaaaaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: #000;
    text-decoration: none;
    cursor: pointer;
}
</style>
