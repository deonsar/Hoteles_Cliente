<template>
  <div class="w3-container w3-card-4" style="text-align: left; min-width:400px; display:inline-block; vertical-align:top">
    <div>
      <h3 style="text-align: center; overflow: hidden;"><strong> Datos Hotel </strong></h3>

      <label class="w3-text" for="nombre"> Nombre: </label>
      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="text" 
      :disabled="!editing && !addingNew" v-model="Hotel.Nombre">
      <br>
     
      <label class="w3-text" for="direccion"> Dirección: </label>
      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="text" 
      :disabled="!editing && !addingNew" v-model="Hotel.Direccion">
      <br>

      <label class="w3-text" for="codigoPostal"> Codigo Postal: </label>
      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="text" 
      :disabled="!editing && !addingNew" v-model="Hotel.CodigoPostal">
      <br>

      <label class="w3-text" for="telefono"> Telefono </label>
      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="number" 
      :disabled="!editing && !addingNew" v-model="Hotel.Telefono">
      <br>

      <label class="w3-text" for="direccion"> Precio: </label>
      <input class="w3-input w3-border" style="background: white; overflow: hidden; text-overflow: ellipsis" type="number" 
      :disabled="!editing && !addingNew" v-model="Hotel.PrecioBase">

      <p class="clasificacion">
          <input id="radio1" type="radio" name="estrellas" :disabled="!editing && !addingNew" v-model="Hotel.Puntuacion" value="5" :checked="Hotel.Puntuacion">
          <label class="star" for="radio1">★</label>
          
          <input id="radio2" type="radio" name="estrellas" :disabled="!editing && !addingNew" v-model="Hotel.Puntuacion" value="4" :checked="Hotel.Puntuacion">
          <label class="star" for="radio2">★</label>
          
          <input id="radio3" type="radio" name="estrellas" :disabled="!editing && !addingNew" v-model="Hotel.Puntuacion" value="3" :checked="Hotel.Puntuacion">
          <label class="star" for="radio3">★</label>
          
          <input id="radio4" type="radio" name="estrellas" :disabled="!editing && !addingNew" v-model="Hotel.Puntuacion" value="2" :checked="Hotel.Puntuacion">
          <label class="star" for="radio4">★</label>
          
          <input id="radio5" type="radio" name="estrellas" :disabled="!editing && !addingNew" v-model="Hotel.Puntuacion" value="1" :checked="Hotel.Puntuacion">
          <label class="star" for="radio5">★</label>
      </p>
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

var httpURL = appConfig.URLHoteles;
var maxInt =  2147483647;

export default {
  components: {
    'app-icon' : AppIcon,
    'infomessage' : InfoMessage
  },

  methods: {
    validateNew: function() {
      let mensaje ='';
      if(this.Hotel.Nombre == '') {
        mensaje = 'El nombre del hotel no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.Direccion == '') {
        mensaje = 'La dirección no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Hotel.CodigoPostal) || this.Hotel.CodigoPostal == '' || this.Hotel.CodigoPostal.length >5) {
        mensaje = 'El código postal no puede estar vacío. Comprobar que el código postal contenga 5 digitos';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Hotel.Telefono) || this.Hotel.Telefono == '' ||  this.Hotel.Telefono.length >9) {
        mensaje = 'El numero de telefono no puede estar vacío. Comprobar que el telefono contenga 9 digitos';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.Puntuacion == 0) {
        mensaje = 'Seleccione la valoración del hotel.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.PrecioBase <= 0 || !this.isNumeric(this.Hotel.PrecioBase)) {
        mensaje = 'El precio base no puede ser 0 .';
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.create();
      }
    },
    validateIdUpdate: function() {
      let mensaje ='';
      if(this.Hotel.Id =='' || this.Hotel.Id < 0) {
        mensaje = 'Seleccione un hotel de la lista.';
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.edit();
      }
    },
    validateIdDelete: function() {
      let mensaje ='';
      if(this.Hotel.Id =='' || this.Hotel.Id < 0) {
        mensaje = 'Seleccione un hotel de la lista.';
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.remove();
      }
    },
    validateUpdate: function() {
      let mensaje ='';
      if(this.Hotel.Nombre == '') {
        mensaje = 'El nombre del hotel no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.Direccion == '') {
        mensaje = 'La dirección no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Hotel.CodigoPostal) || this.Hotel.CodigoPostal == '' || this.Hotel.CodigoPostal.length >5) {
        mensaje = 'El código postal no puede estar vacío. Comprobar que el código postal contenga 5 digitos';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Hotel.Telefono) || this.Hotel.Telefono == '' ||  this.Hotel.Telefono.length >9) {
        mensaje = 'El numero de telefono no puede estar vacío. Comprobar que el telefono contenga 9 digitos';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.Puntuacion == 0) {
        mensaje = 'Seleccione la valoración del hotel.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Hotel.PrecioBase <= 0 || !this.isNumeric(this.Hotel.PrecioBase)) {
        mensaje = 'El precio base no puede ser 0 .';
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
      this.HotelCopia.Nombre = this.Hotel.Nombre;
      this.HotelCopia.Direccion = this.Hotel.Direccion;
      this.HotelCopia.CodigoPostal = this.Hotel.CodigoPostal;
      this.HotelCopia.Telefono = this.Hotel.Telefono;
      this.HotelCopia.Puntuacion = this.Hotel.Puntuacion;
      this.HotelCopia.PrecioBase = this.Hotel.PrecioBase;

      this.Hotel.Nombre = '';
      this.Hotel.Direccion = '';
      this.Hotel.CodigoPostal = '';
      this.Hotel.Telefono = '';
      this.Hotel.Puntuacion = '';
      this.Hotel.PrecioBase = '';

      this.addingNew = true;
    },
    discardNew: function () {
      this.Hotel.Nombre = this.HotelCopia.Nombre;
      this.Hotel.Direccion = this.HotelCopia.Direccion;
      this.Hotel.CodigoPostal = this.HotelCopiaH.CodigoPostal;
      this.Hotel.Telefono = this.HotelCopia.Telefono;
      this.Hotel.Puntuacion = this.HotelCopia.Puntuacion;
      this.Hotel.PrecioBase = this.HotelCopia.PrecioBase;

      this.addingNew = false;
    },
    edit: function () {
      this.HotelCopia.Nombre = this.Hotel.Nombre;
      this.HotelCopia.Direccion = this.Hotel.Direccion;
      this.HotelCopia.CodigoPostal = this.Hotel.CodigoPostal;
      this.HotelCopia.Telefono = this.Hotel.Telefono;
      this.HotelCopia.Puntuacion = this.Hotel.Puntuacion;
      this.HotelCopia.PrecioBase = this.Hotel.PrecioBase;

      this.editing = true;
    },
    discard: function () {
      this.Hotel.Nombre = this.HotelCopia.Nombre;
      this.Hotel.Direccion = this.HotelCopia.Direccion;
      this.Hotel.CodigoPostal = this.HotelCopiaH.CodigoPostal;
      this.Hotel.Telefono = this.HotelCopia.Telefono;
      this.Hotel.Puntuacion = this.HotelCopia.Puntuacion;
      this.Hotel.PrecioBase = this.HotelCopia.PrecioBase;

      this.editing = false;
    },
    cleanForm: function() {
      this.Hotel.Nombre = '';
      this.Hotel.Direccion = '';
      this.Hotel.CodigoPostal = '';
      this.Hotel.Telefono = '';
      this.Hotel.Puntuacion = '';
      this.Hotel.PrecioBase = '';
      this.Hotel,Id = '';
    },
    create: function () {
      var _this = this;
      $.ajax(
        {
          url : httpURL,
          type: "POST",
          data: {
            Nombre: this.Hotel.Nombre,
            Direccion: this.Hotel.Direccion,
            CodigoPostal: this.Hotel.CodigoPostal,
            Telefono: this.Hotel.Telefono,
            Puntuacion: this.Hotel.Puntuacion,
            PrecioBase: this.Hotel.PrecioBase
          }

        })
        .done(function(data) {
          EventBus.$emit('updateListHotel');
          let mensaje ='Hotel añadido con éxito.';
          EventBus.$emit('showMessage', mensaje);
          _this.addingNew = false;
        })
        .fail(function(data) {
          let mensaje = 'No se puede crear el hotel. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
      },
      update: function () {
        var _this = this;
        $.ajax(
          {
            url : httpURL + this.Hotel.Id,
            type: "PUT",
            data: {
              Id: this.Hotel.Id,
              Nombre: this.Hotel.Nombre,
              Direccion: this.Hotel.Direccion,
              CodigoPostal: this.Hotel.CodigoPostal,
              Telefono: this.Hotel.Telefono,
              Puntuacion: this.Hotel.Puntuacion,
              PrecioBase: this.Hotel.PrecioBase
            }
          })
          .done(function(data) {
            EventBus.$emit('updateListHotel');
            let mensaje ='Hotel actualizado con éxito.';
            EventBus.$emit('showMessage', mensaje);
            _this.cleanForm();
            _this.editing = false;
          })
          .fail(function(data) {
            let mensaje = 'No se pudo actualizar el hotel. Revise su conexión a Internet.';
            EventBus.$emit('showMessage', mensaje);
          });
        },
        remove: function () {
          var _this = this;
          $.ajax(
            {
              url : httpURL + this.Hotel.Id,
              type: "DELETE",
              data: {Id: this.Hotel.Id}
            })
            .done(function(data) {
              EventBus.$emit('updateListHotel');
              _this.cleanForm();
              let mensaje ='Hotel eliminado con éxito.';
              EventBus.$emit('showMessage', mensaje);
              _this.editing = false;
            })
            .fail(function(data) {
              let mensaje = 'No se pudo eliminar el hotel. Revise su conexión a Internet.';
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

                _this.Hotel = data;
              })
              .fail(function(data) {
                let mensaje = 'No se puede cargar los datos. Revise su conexión a Internet.';
                EventBus.$emit('showMessage', mensaje);
              });
            },
          },

          data: function () {
            return {
              editing: false,
              addingNew: false,
              complejidad: { Alta: 'Alta', Media: 'Media', Baja: 'Baja'},
              Hotel: {
                Id: '',
                Nombre: '',
                Direccion: '',
                CodigoPostal: '',
                Telefono: '',
                Puntuacion: '',
                PrecioBase: ''
              },
              HotelCopia: {
                Id: '',
                Nombre: '',
                Direccion: '',
                CodigoPostal: '',
                Telefono: '',
                Puntuacion: '',
                PrecioBase: ''
              }
            }
          },

          mounted: function() {
            EventBus.$on('hotelSelected', function(id) {
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

label:hover,
label:hover ~ .star {
  color: orange;
}

input[type="radio"]:checked ~ label {
  color: orange;
}
</style>