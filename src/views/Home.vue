<template>
<div class="home mt-5" >
  <v-container v-if="loading" class="progress">
    <div class="text-xs-right">
      <v-progress-circular
        indeterminate
        :size="50"
        :width="5"
        color="pink"
        right>
      </v-progress-circular>
    </div>
  </v-container>
  <v-container v-else>
 <v-spacer></v-spacer>
  <v-card elevation="2" class="mx-auto"
      max-width="450">
      <v-list >
        <draggable v-model="items" class="v-tabs__container ml-5" >
          <v-list-item
            v-for="item in items"
            :key="item.id"
            ripple
          >
        <v-list-item-icon>
          <v-icon
            color="pink"
          >
            adjust
          </v-icon>
        </v-list-item-icon>

        <v-list-item-content>
          <v-list-item-title v-text="item.item"></v-list-item-title>
        </v-list-item-content>
        
        <v-list-item-avatar>
           <router-link
             :to="{
               name:'edit',
               params: { id: item.id }
             }"
             > <v-icon color="pink" >edit</v-icon>
                    </router-link>
        </v-list-item-avatar>

        <v-list-item-avatar>
          <v-icon color="grey" @click="deleteTodo(item)">delete</v-icon>
        </v-list-item-avatar>

      </v-list-item>
      </draggable>
    </v-list>
  </v-card>
  </v-container>
</div>
</template>

<script>
import axios from 'axios'
import draggable from "vuedraggable";

export default {
  components: {
     draggable
 },
  data() {
    return {
      item:'',
      items: [],
      search: "",
      loading:true
    }
  },
  created() {
    this.all();
  },
    
  methods: {
    validate: function () {
        this.$refs.form.validate()
      },
    all: function () {
        axios.get('https://backend-d.herokuapp.com/api/todo/')
            .then( response => {
                this.items = response.data
                this.loading = false
            });
    },
       deleteTodo: function(items) {
      if (confirm("Delete " + items.item)) {
        axios.delete(`https://backend-d.herokuapp.com/api/todo/${items.id}`)
          .then(response => {
            console.log(response);
            this.all();
          });
      }
    },
}
}
</script>

<style >
.progress{
  margin-top:50px;
  margin-left:550px;
}
</style>