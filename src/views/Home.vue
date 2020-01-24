<template>
<div>
  <h1 class="text-center display-2">Valor del Dolar</h1>
  <v-row :wrap="true">
    <v-col xs12>
      <v-card>
        <v-date-picker 
        v-model="fecha" 
        color="green lighten-1" 
        full-width first-day-of-week="1" 
        locale="es-cl" 
        :min="minimo"
        :max="maximo"
        @change="getDolar(fecha)"
        ></v-date-picker>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 font-italic font-weight-medium text-center">
          {{ valor }}
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
  
</div>  
</template>

<script>

import axios from "axios";

export default {
  name: 'home',
  data () {
    return {
      fecha: new Date().toISOString().substr(0,10),
      minimo: '1984',
      maximo: new Date().toISOString().substr(0,10),
      valor:  null
    }
  },
  methods:{
      /* async getDolar () {
        let datos = await axios.get('https://mindicador.cl/api/dolar')
        console.log(datos)
      } */
      getDolar (dia) {
        
        let ddmmyyy = dia.split(['-']).reverse().join(['-'])
        try {
          axios.get(`https://mindicador.cl/api/dolar/${ddmmyyy}`)
        .then(response => {
          if(response.data.serie.length > 0){
            this.valor = response.data.serie[0].valor
            }else{
              this.valor = 'Sin Resultados :('
            }
        }) 
        } catch (error) {
          console.log(error)
        }
        
      }
    },
    created () {
      this.getDolar(this.fecha)
    }
}
</script>
