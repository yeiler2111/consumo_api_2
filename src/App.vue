<template>
  <div class="imagen" id="app">
  
    <b-container  class="justify-content-center">
      <b-row >
        <b-col > <button class="btn btn-primary gap-2"  @click="verActors">ver actores</button></b-col>
        <b-col > <button class="btn btn-primary "  @click="verMovies">ver peliculas</button></b-col>
      </b-row>  
        
    </b-container>
      
    <nuevo v-if="viewMovies"></nuevo>
      <actors-table v-if="viewActors"></actors-table>
  </div>
</template>

<script>
// import perros from "./components/imagenesPerros.vue";
// import actor from "./components/TablaInformacion.vue";

import ActorsTable from "@/components/ActorsTable.vue"
import nuevo from "@/components/MoviesForActors.vue"
export default {
  name: "App",
  components: {
   
    ActorsTable,
    nuevo
  },
  data(){
    return{
      viewActors:false,
      viewMovies:false,
   
    }
  },
  
  methods:{
    verActors(){
      this.viewActors= !this.viewActors 
    },
    verMovies(){
      this.viewMovies= !this.viewMovies
    }
    }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
  margin-top: 0;

  
}

.imagen{
  background-image: url("@/assets/373.jpg");
  background-size: cover;
  background-position: cover;
  width: 100%;
  height: 100vh;
  object-fit:cover;
  background-repeat: none;
  

}

</style>
