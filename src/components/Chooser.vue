<template>
  <div>
    
    <button class="btn btn-default btn-lg " title="Hoteles" @click="chooseOption(options.Hoteles)">
      <app-icon img="th-list"></app-icon> Lista Hoteles
    </button>

    <button class="btn btn-default btn-lg" title="Reservas" @click="chooseOption(options.Reservas)">
     <app-icon img="duplicate"></app-icon> Reservas Solicitadas
    </button>    
    
  </div>
</template>

<script>
import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'

export default {
  components: {
    'app-icon' : AppIcon
  },

  data: function() {
    return {
      options: {
        Hoteles: 'hoteles',
        Reservas: 'reservas',
        
      }
    }
  },
  methods: {
    chooseOption: function(opcion){
      EventBus.$emit('chooseOption', opcion);
    }
  }
}

</script>

