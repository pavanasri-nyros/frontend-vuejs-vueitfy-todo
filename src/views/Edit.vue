<template>
    <div class="pt-5 align">
      <v-container v-if="loading" class="progresss">
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
           <v-card elevation="30" class="ml-5" max-width="550" large
  raised>
           <h2 class="ml-3 pink--text">Edit Form</h2>
         <v-col class="close_align" >
           <v-btn 
            icon
            color="pink"
            router
            to="/home"
            class="closer"
          >
          <v-icon>cancel</v-icon>
          </v-btn>
          </v-col>
        <form @submit.prevent="update" method="post">
         <v-text-field
          v-model="item.item"
          :counter="20"
          :rules="itemRules"
          name="item"
          id="item"
          required
          class="ml-3 mr-3"
        ></v-text-field>
       <v-btn
           :disabled="!valid"
           color="pink"
           class="mr-4 ml-3 mb-3 white--text"
           type="submit"
           
        >
          <div>Update</div>
        </v-btn>
    </form>
  </v-card>
  </v-container>

  </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            item:'',
            loading:true,
            valid: true,
            itemRules: [
               v => !!v || 'Item is required',
               v => (v && v.length <= 20) || 'Item must be less than 20 characters',
            ],
        }
    },
     mounted() {
    axios
      .get(`https://backend-d.herokuapp.com/api/todo/` + this.$route.params.id)
      .then(response => {
        console.log(response.data);
        this.item = response.data;
        this.loading = false
      });
  },
   methods: {
    update: function() {
        axios.put(`https://backend-d.herokuapp.com/api/todo/${this.item.id}/`,
            this.item
          )
          .then(response => {
            console.log(response);
            this.$router.push("/");
            this.loading = false
          });
    }
  }
}
</script>

<style>
.align {
  margin-left: 340px;
}
.closer {
  margin-left:486px;
  margin-top: -60px;
}

.progresss{
  margin-top:50px;
  margin-left:250px;
}


</style>