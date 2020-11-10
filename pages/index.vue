<template>
  <v-layout column>

  <v-row>


      <v-col  cols="4">
          <v-card
            color="teal"
            dark
          >
            <v-card-title class="headline">Doctors</v-card-title>
            <v-card-actions>
              <v-btn text>{{dl}}</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col  cols="4">
          <v-card
            color="teal"
            dark
          >
            <v-card-title class="headline">Cases</v-card-title>
            <v-card-actions>
              <v-btn text>{{pt}}</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col  cols="4">
          <v-card
            color="teal"
            dark
          >
            <v-card-title class="headline">Field Officers</v-card-title>
            <v-card-actions>
              <v-btn text>{{fo}}</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
 
  
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data (){
    return {
      dl:'',
      pt:'',
      fo:'',
        items: [
        // {    
         
        //   title: 'Doctors',
        //   total: '05',
        // },
        // {    
         
        //   title: 'Doctors',
        //   total: '05',
        // },
        // {    
         
        //   title: 'Doctors',
        //   total: '05',
        // },
      
      ],
    }
  },
  created(){

   this.$axios.get('doctors').then(res => {

          this.dl = res.data.data.length
  
        });

  this.$axios.get('patient').then(res => {

          this.pt = res.data.length
  
        });  

  this.$axios.get('field_officers').then(res => {

          this.fo = res.data.data.length
  
        });

  }
}
</script>
