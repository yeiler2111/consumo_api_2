<template>
    <div>
        <span style="border-radius: 5px;font-weight: bold; font-size:3rem ; color: white; background-color: black;">Peliculas Estrenos</span>
        <b-table head-row-variant="dark" id="my-table" :per-page="perPage" :current-page="currentPage"  small fixed bordered :items="destrucMovie" :fields="fields" striped responsive="sm">
            <template #cell(ver_actores)="row">
                <b-button size="sm" @click="getinfo(row)" class="mr-2">
                {{ row.detailsShowing ? 'Hide' : 'Show'}} actors
                </b-button>
            </template>
            <template #row-details>
                <b-container>
                    <b-row class="justify-content-md-center">    
                        <b-card-group v-if="selectedMovie">
                            <b-card  v-for="items in trues" :key="items.id"
                            :title="items.node.name.nameText.text"
                            :img-src="items.node.name.primaryImage && items.node.name.primaryImage.url"
                            img-alt="Image"
                            img-top
                            tag="article"
                            style="max-width: 20rem;"
                            class="mb-2 text-center"
                            >  
                        </b-card>
                        </b-card-group>
                    </b-row>
                </b-container>
                    
            </template>
        </b-table>
        <b-pagination class="justify-content-center mt-5"
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
            actoresForMovies:[],
            actores:[],
            idMovies:[],
            movies:[],
            destrucMovie:[],
            fields:['titulo','year','ver_actores'],
            trues:[],
            perPage: 5,
           currentPage: 1,
           selectedMovie:null
        }
    },
    computed:{
        rows(){
            return 10
        }
        
    },
    methods:{
        async getActorsAndMovies(){
            try{
                let {data} = await MovieServices.getActors()
                this.actoresForMovies=data.results
                for(let i=0;i<this.actoresForMovies.length;i++){
                    this.idMovies.push(this.actoresForMovies[i].id)
                }
            }catch(e){
                console.log(e)
            }
           
           this.actoresForMovies.forEach(element => {
                this.actores.push(element.cast.edges)
                
           });
            
           
           
        },  
        async getTitle() {
            let peli
            try {
                for (let i = 0; i < this.idMovies.length; i++) {
                const id = this.idMovies[i]
                const response = await MovieServices.getMovieMainActors(id)
                this.movies.push(response.data.results);
                }
                
                
            } catch (error) {
                console.error(error);
            }
            


            for(let i=0;i<this.movies.length;i++){
                peli={
                    id:this.movies[i].id,
                    titulo:this.movies[i].originalTitleText.text,
                    imagen:this.movies[i].primaryImage && this.movies[i].primaryImage.url ,
                    year:this.movies[i].releaseYear.year
                }
                this.destrucMovie.push(peli)
            }
            
        },
        getinfo(row){
            
            if (this.selectedMovie === row.item) {
                row.toggleDetails()
                this.selectedMovie = null
                return;
            }
            if (this.selectedMovie !== null) {
                this.selectedMovie.detailsShowing = false
                return;
            }

            row.toggleDetails();
            this.selectedMovie = row.item;
            
        
            for(let i=0;i< this.actoresForMovies.length;i++){
                if(this.actoresForMovies[i].id == this.selectedMovie.id){
                    this.trues=this.actoresForMovies[i].cast.edges
                }
           }
        

        }
    },
    async created(){
        await this.getActorsAndMovies()
        await this.getTitle()

    }
} 

</script>