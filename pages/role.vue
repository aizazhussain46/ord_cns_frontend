<template>
  <v-data-table
    :headers="headers"
    :items="data"
    sort-by="role"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Role</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <!-- <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
          </template> -->
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field v-model="editedItem.role" label="role"></v-text-field>
                  </v-col>
                
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <!-- <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon> -->
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
          text: 'role',
          sortable: true,
          value: 'role',
        },
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      editedIndex: -1,
      editedItem: {
       role:'',
      },
      defaultItem: {
       role:'',
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

      this.$axios.get('role').then(res => {

          this.data = res.data
        console.log(res.data)  

        });

      },

      editItem (item) {
        this.editedIndex = this.data.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.data.indexOf(item)
         confirm('Are you sure you want to delete this item?') && 
         this.$axios.delete('role/'+item.id)
            .then((res) => {
     
              const index = this.data.indexOf(item)
              this.data.splice(index, 1)
            
            });
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
       //   Object.assign(this.data[this.editedIndex], this.editedItem)

            this.$axios.put('role/' + this.editedItem.id, {
            role: this.editedItem.role
            })
            .then(res => {

            const index = this.data.findIndex(item => item.id == this.editedItem.id)
            this.data.splice(index, 1,{
            id:this.editedItem.id,
            role:this.editedItem.role
            });
   
              this.close()
     
            })
            .catch(error => console.log(err));


        } else {
          
              this.$axios.post('role',{role:this.editedItem.role})
              .then((res) => {
            
              this.data.push(res.data.data)
              this.close()
              
            
            });
        }
     
      },
    },
  }
</script>