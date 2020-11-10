<template>
  <v-data-table
    :headers="headers"
    :items="data"
    sort-by="followup"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Follow Ups</v-toolbar-title>        
      </v-toolbar>
    </template>
    
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Patient Name',
          sortable: true,
          value: 'p_name',
        },
        {
          text: 'CNIC No',
          sortable: false,
          value: 'p_cnic',
        },
        {
          text: 'Mobile No',
          sortable: true,
          value: 'p_mobile_no',
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status',
        },
        {
          text: 'Date',
          sortable: false,
          value: 'created_at',
        },

      ],
      data: [],
      defaultItem: {
       p_name:'',
       p_cnic:'',
       p_mobile_no:'',
       status:'',
       created_at:'',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {

      this.$axios.get('patient').then(res => {

          this.data = res.data

        });

      },


      
    },
  }
</script>