<template>
    
    <div class="overflow-auto">
        <h1 style="font-weight: bold; font-size:3rem ;">Actores</h1>
        <b-table   id="my-table" :per-page="perPage" :current-page="currentPage"  small fixed bordered :items="actores" :fields="fields" responsive="sm">
            <template #cell(ver_Peliculas)="row">
                <b-button size="sm" @click="getMovies(row)" class="mr-2">
                {{ row.detailsShowing ? 'Hide' : 'Show'}} Movies
                </b-button>
            </template>

            <template #row-details>
                <div class="d-flex justify-content ">
                    <b-card v-for="items in arrayPeliculas" :key="items.id"
                        :title="items.originalTitleText.text
"
                        :img-src="items.primaryImage.url"
                        img-alt="Image"
                        img-top
                        tag="article"
                        style="max-width: 20rem;"
                        class="mb-2"
                        >
                        <b-card-text>
                            <h3>
                               año de lanzamiento fue en {{items.releaseYear.year}} y su titulo es {{items.titleText.text}}                             
                            </h3> 
                          
                        </b-card-text>

                        
                    </b-card>
                </div>
                    
            </template>
           
        </b-table>
        <b-pagination class="justify-content-center"
                v-model="currentPage"
                :total-rows="rows"
                :per-page="perPage"
                aria-controls="my-table">
        </b-pagination>
    </div>  
</template>

<script>
import MovieServices from "./../services/MovieService";
export default{
   data(){
    return{
        actores:[],
        fields:['primaryName','birthYear','deathYear','ver_Peliculas'],
        arrayPeliculas:[],
        perPage: 10,
        currentPage: 1,
    }
   },computed:{
    rows(){
        return 50
    }
   }
   ,
   methods:{
        async getActores(){
            try {
                var { data } = await MovieServices.getActores();
                this.actores = data.results;
            }catch (error) {
                console.log(error);
    
            }
           
        },
        getMovies(row){
            row.toggleDetails()
           this.arrayPeliculas=[]
           let arrayCodigos= row.item.knownForTitles.split(',')
         
           for(let i=0;i<arrayCodigos.length;i++){
                MovieServices.getMovieMainActors(arrayCodigos[i]).then(Response=>{
                this.arrayPeliculas.push(Response.data.results)  
           })
           }
          
        },
       
    },
    async created(){
        await this.getActores()
    
         
   }
   
}
</script>