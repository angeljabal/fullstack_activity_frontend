<template>
    <div>
       <button class="btn btn-success float-right" @click="onNew">
            New Movie
       </button>

       <h5>Movies View</h5>
       <hr>

       <form action="" @submit.prevent="onSave">
            <b-form-group label="Title" label-for="title">
            <b-form-input id="title" v-model="movie.title" trim></b-form-input>
            </b-form-group>

            <b-form-group label="Description" label-for="description">
            <b-form-input id="description" v-model="movie.description" trim></b-form-input>
            </b-form-group>

            <b-form-group label="Genre" label-for="genre">
            <b-form-input id="genre" v-model="movie.genre" trim></b-form-input>
            </b-form-group>

            <b-form-group label="Year" label-for="year">
            <b-form-input id="year" type="number" v-model="movie.year" trim></b-form-input>
            </b-form-group>

            <b-form-group label="Status" label-for="status">
            <b-form-select v-model="movie.status" :options="statuses"></b-form-select>
            </b-form-group>

            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger" type="button" @click="onDelete" v-if="movie.id">Delete</button>
            </b-form-group>

       </form>

    </div>
</template>

<script>
export default {
    props: {
        movie: {}
    },
    data() {
        return{
            statuses: [
                {value: 'unwatched', text: 'Unwatched'},
                {value: 'unfinished', text: 'Unfinished'},
                {value: 'Completed', text: 'Completed'},
            ]
        }
    },
    methods: {
        async onSave() {
           try{
                if(!this.movie.id){
                    await this.$axios.post('/movies', this.movie)
                }else{
                    await this.$axios.put('/movies/' + this.movie.id, this.movie)
                }
                this.$emit('saved');
           }catch(err){
               alert(err.response.data.message)
           }
    },
    onNew(){
        this.$emit('newMovie')
    },
    async onDelete(){
        try{
            this.$axios.delete('/movies/' + this.movie.id)
            this.$emit('deleted')
        }catch(err){
            alert(err.response.data.message)
        }
    }
  }
}
</script>