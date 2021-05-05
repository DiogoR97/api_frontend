<template>
    <v-container>
      <input type="text" v-model="pesquisa" >
          <button @click="procura(pesquisa)">Search</button>
          <hr/>
        <div class="row">
         
        <!-- Lista favoritos -->
        <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>

            <div class="colunaFav">

                <div v-if="favoritos.length >0">
                  <h3>Favoritos</h3>
                  <div v-for="(fav, index) in favoritos" :key="index">
                      {{fav.data[0].title}} <v-icon x-small @click="removeFav(index)">mdi-close-circle-outline</v-icon>
                  </div>
                </div>

          </div>
<!-- Lista Produtos -->
    
      <div class="colunaProduct">
        <v-card max-width="374"
            class="mx-auto my-12" v-for="(item, index) in info" :key="index">
                <v-card-title>{{item.data[0].Title}}</v-card-title>
                <v-card-text>
              <v-row>
                  {{item.data[0].description}}
              </v-row>
              </v-card-text>
                <v-img
              height="150"
              :src="item.links[0].href"
            > 
            </v-img>
              <v-card-actions>
                <v-btn
                  color="deep-purple "
                  text
                  @click="favorito(item)"
                >
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
        </div>
  </div>
   </v-container>
</template>

<style>
.row {
  display: flex;
}

.colunaProduct {
  flex: 60%;
  padding: 10px;
}

.colunaFav {
  flex: 40%;
  padding: 10px;
}
</style>



<script>
import axios from "axios";

export default {
  data() {
    return {
      info: null,
      favoritos: [],
      vertical: true,
      multiLine: true,
      snackbar: false,
      text: `Esse item já foi adicionado aos favoritos`,
    };
  },
  mounted() {
    var that = this;
    axios
      .get("https://images-api.nasa.gov/search?q=earth&media_type=image")
      .then(response => (this.info = response.data.collection.items));
    console.log(that.info);
  },
  methods: {
    favorito(item) {
      if (this.favoritos.indexOf(item ) === -1){
        this.favoritos.push(item);
        
      }else {
        this.snackbar = true;
      }
      console.log(this.favoritos);
    },

    removeFav(item){
      this.favoritos.splice(item, 1)

    },

    procura(pesquisa) {
      axios.get("https://images-api.nasa.gov/search?q="+ pesquisa + "&media_type=image" )
      .then(response => (this.info = response.data.collection.items));
    }
  },
};
</script>
