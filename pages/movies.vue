<template>
    <div>
        <Navbar/>
    <div class="container">
            <h1>My Movies List</h1>
            <div class="row">
                <div class="col-6">
                    <h5>Movies List</h5>
                    <ul class="list-group">
                        <li class="list-group-item list-group-item-action" v-for="movie in movies" :key="movie.id" @click="onSelected(movie)">
                            {{movie.title}} <span class="float-right">{{movie.year}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <MovieView :movie="selectedMovie" @saved="onChanges" @newMovie="onNew" @deleted="onChanges"/>
                </div>
            </div>
        </div>
    </div>
   
</template>

<script>
export default {
    data(){
        return{
            movies: [],
            selectedMovie: {}
        }
    },

    methods: {
        async getAll(){
            await this.$axios.get('/movies')
            .then((res)=>{
                if(res.status==200){
                    this.movies = res.data
                }
            })
        },
        onChanges(){
            this.getAll()
            this.selectedMovie = {}
        },
        onSelected(movie){
            this.selectedMovie = movie
        },
        onNew(){
            this.selectedMovie = {}
        },
        onDeleted(){
            this.getAll()
            this.selectedMovie = {}
        }
    },
    created(){
        this.getAll()
    }
}
</script>