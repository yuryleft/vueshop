<template>
  <div id="app" >
        <div class="wrapper" @click="showViewed = false">

            <header class="header">
                Поиск фильмов 
                <a class="header-link" href="#favorite" >К избранному</a>
            </header>
            <div class="common-block">


                <div class="search">

                    <div class="search-title">{{header}} </div>
            
                    <div class="search-console">
                        <input type="text" id="search-input" style="color: #595959" v-model.trim="request" @focus="request=''">
                    </div>
       
                    <div class="film-card" v-for="film in filteredFilms" :key="film.id" >
                        <div class="film-card-img">
                         <img src="http://placehold.it/145" alt="">
                        </div>
                        <div class="film-card-content">
                            <p>{{film.title}}</p>
                            <p>Дата выпуска: {{film.release_date}}</p>
                            <p>Описание фильма: {{film.opening_crawl}}</p>
                            <button class="btn waves-effect" @click.stop="addFilmToViewed(film)">Добавить в избранное</button>
                        </div>
                    </div>

                </div>
       
                <div class="favorite" id="favorite">

                    <div class="favorite-top" @click.stop="showViewed = !showViewed" >
                    <div>Избранное</div>
                    <a class="favorite-btn btn-floating waves-effect"  >
                    <i class="material-icons">turned_in_not</i>
                    </a>
                    </div>

                    <div class="favorite-menu" v-show=showViewed @click.stop="" >
                        
                        <div class="favorite-menu-item" v-for="film in viewedFilms" :key="film.id">

                         <div>{{film.title}}</div>
                         <div>{{film.release_date}}</div>
                        <button class="btn btn-small" @click.stop="deleteFilm(film)">Удалить</button>
                        
                        </div>
                        
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
 let initheader="Введите запрос";
        export default {
           
           data(){
               return {
               request:initheader,
               films:[],
               showViewed:false,
               viewedFilms:[]                   
           }
           },
           computed:{
               header(){               
                return this.request === "" || this.request === initheader ? "Ничего не ищем" : 'Идет поиск: '+ this.request;
               },
               filteredFilms(){
                   if(this.request !== initheader){
                   return this.films.filter( el => el.title.toLowerCase().includes(this.request.toLowerCase()) );
                   } else {
                       return this.films;
                }

               }
               },

               methods: {
                addFilmToViewed(film){
                    if ( !this.viewedFilms.find( el => el.title === film.title) ){
                        this.viewedFilms.push(film);
                    }
                    this.showViewed = true;

                    setTimeout( () => {
                        this.showViewed = false;
                    }, 500)
                },
                deleteFilm({title}){
                    this.viewedFilms = this.viewedFilms.filter( el => el.title !== title);
                }
            },
           
           async mounted(){
                this.films = await fetch("https://swapi.dev/api/films/").then(res => res.json()).then(data => data.results);
                
           }
        
        }
</script>


<style >

</style>
