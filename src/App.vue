<template>
  <div id="app">
    <master-chooser></master-chooser>
    <h1>{{msg}}</h1> 
    <h2 v-show="opcion == 'hoteles'"><strong>{{ title.Hoteles }}</strong></h2>
    <maestro-hoteles v-show="opcion == 'hoteles'"></maestro-hoteles>    
    <detalle-hoteles v-show="opcion == 'hoteles'"></detalle-hoteles>

    <h2 v-show="opcion == 'reservas'"><strong>{{ title.Reservas }}</strong></h2>
    <maestro-reservas v-show="opcion == 'reservas'"></maestro-reservas>
    <detalle-reservas v-show="opcion == 'reservas'"></detalle-reservas>

    
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
      opcion:"hoteles"
    }
  },
  
  mounted: function(){
    EventBus.$on('chooseOption', function(opcion) {
      this.opcion = opcion;
    }.bind(this));
  }
}
</script>

<style lang="scss">
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
</style>
