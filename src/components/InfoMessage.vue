<template>
  <div  id="myModal" class="modal-message w3-container " style="display:inline-block;" v-if="show">
     
    <div class="modal-message-content  w3-animate-top ">
        <span class="close" @click="show = false">&times;</span>
        <h4 style="text-align: center; vertical-align: middle; margin-top:40px;"> {{mensaje}} </h4>
    </div>

  </div>

</template>

<script>
import EventBus from './event-bus.js'

export default {
  data: function() {
    return {
      show: false,
      mensaje: ''
    }
  },

  methods: {
    showMessage: function(message){
      this.mensaje = message;
      this.show = true;
    },
  },

  mounted: function() {
    EventBus.$on('showMessage', function(message) {
    this.showMessage(message);
    
    }.bind(this));
  }

};

</script>

<style>

/*.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
{
  transform: translateX(10px);
  opacity: 0;
}*/





.modal-message{
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
.modal-message-content {
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
