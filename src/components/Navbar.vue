<template>
  <nav>
    <v-app-bar>

        <v-icon 
          left 
          large
          color="pink"
        >cloud_done
        </v-icon>

          <v-toolbar-title class="text-uppercase pink--text">
               <span class="font-weight-light">Item</span>
               <span>List</span>
          </v-toolbar-title>

           <v-spacer></v-spacer>
            <v-tooltip bottom>
             <template v-slot:activator="{ on, attrs }">
            <v-btn text color="pink" class="white--text mr-3" @click="dialog = !dialog" router to="/create" v-bind="attrs" v-on="on">
               <span @click="reset()">Add Item</span>
            </v-btn>
            </template>
             <span>We can add item</span>
           </v-tooltip>

           <v-tooltip bottom>
             <template v-slot:activator="{ on, attrs }">
               <v-btn 
                    icon
                    color="pink"
                    router to="/home"
                >
                <v-icon router to="/home" v-bind="attrs" v-on="on">home</v-icon>
               </v-btn>
              </template>
             <span>Home</span>
            </v-tooltip>

           <v-tooltip bottom>
             <template v-slot:activator="{ on, attrs }">
             <v-btn 
                icon
                color="pink"
                class="ml-2"
                v-bind="attrs" v-on="on"
                @click="sheet = !sheet"
            >
             <v-icon>info</v-icon>
            </v-btn>
             </template>
             <span>Info</span>
            </v-tooltip>

  <!-- Bottom info page -->
         <v-bottom-sheet v-model="sheet">
           <v-sheet
             class="text-center"
             height="400px"
           >
             <v-btn
               text
               color="pink"
               @click="sheet = !sheet"
               class="bottomclose"
             >
               <v-icon large>cancel</v-icon>
             </v-btn>
             <div>
              
               <v-list>
                 
                 <v-list-item v-for="info in info" :key="info.info">
                   <v-btn
                      text
                      color="pink"
                    >
                      <v-icon>info</v-icon>
                    </v-btn>
                   {{info.info}}
                   <v-list-item-icon>
                     <v-icon>
                       {{info.icon}}
                     </v-icon>
                   </v-list-item-icon>
                 </v-list-item>
                </v-list>
             </div>
           </v-sheet>
         </v-bottom-sheet>
    </v-app-bar>
    
      <!-- Create form -->
       <v-dialog
          v-model="dialog"
          max-width="500px"
          white
        >
        <v-col class="white">
          <v-col class="close_align" @click="resetValidation();">
           <v-btn 
            icon
            color="pink"
            @click="dialog = false"
            class="close"
            router
            to="/home"
          >
            <v-icon>cancel</v-icon>
          </v-btn>
          </v-col>
           <v-form
             ref="form"
             v-model="valid"
             lazy-validation
             @keyup.enter="create"
             @submit.prevent="create" method="post"
           >
             <v-text-field
               v-model="item"
               :counter="20"
               :rules="itemRules"
               label="Add Item"
               id="item"
               name="item"
               required
             ></v-text-field>
              <v-spacer></v-spacer>
               <v-btn
                   :disabled="!valid"
                   color="pink"
                   class="mr-4  white--text"
                   @click="dialog = false"
                   type="submit"
                >
               <div @click="validate();">Submit</div>
            </v-btn>
           </v-form>
          </v-col>
        </v-dialog>
        <!-- End of create form -->
   </nav>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      dialog: false,
       sheet: false,
      link: [
         {home: '/home'}
      ],
      valid: true,
      item: '',
      itemRules: [
        v => !!v || 'Item is required',
        v => (v && v.length <= 20) || 'Item must be less than 20 characters',
      ],
      info:[
        {icon:'add', info:'We can Add item into the app'},
        {icon:'delete', info:'We can Delete the item by clicking on the delete icon'},
        {icon:'create', info:'We can Update the Item'},
        {icon:'visibility', info:'We can Read the item'},
        {icon:'drag_handle', info:'Reorder the items by dragging'},

      ]
    }
  },
  methods: {
      validate () {
        this.$refs.form.validate()
      },
       resetValidation () {
        this.$refs.form.reset()
        this.$refs.form.resetValidation()
      },
      create () {
       axios.post('https://backend-d.herokuapp.com/api/todo/',{item: this.item})
            .then(response => {
                 console.log(response);
                 this.$router.push("/");
               });
        }
       
    }
  }

</script>

<style >
.close {
  margin-left:440px;
}
.close_align{
  margin-top: -10px;
}
.bottomclose{
  margin-left: 1221px;
}
</style>
