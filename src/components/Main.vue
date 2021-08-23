<template>
  <div id="app" >
        <div class="wrapper" @click="showViewed = false">

            <header class="header">
                Поиск квартир
                <a class="header-link" href="#favorite" >К избранному</a>
            </header>
            <div class="common-block">


                <div class="search">

                    <div class="search-title">{{header}} </div>
            
                    <div class="search-console">
                        <input type="text" id="search-input" style="color: #595959" v-model.trim="request" @focus="request=''">
                    </div>
       
                    <div class="flat-card" v-for="flat in filteredflats" :key="flat.id" >
                        <div class="flat-card-img">
                         <img src="http://placehold.it/145" alt="">
                        </div>
                        <div class="flat-card-content">
                            <div>{{flat.attributes.title}}</div>
                            <div>Кол-во комнат : {{flat.attributes.rooms}}</div>
                            <div>Адресс : {{flat.attributes.address.city}}, {{flat.attributes.address.street}} {{flat.attributes.address.house}}, кв. {{flat.attributes.address.room}}</div>
                            <div>Площадь : {{flat.attributes.area}} {{flat.attributes.unit}} </div>
                            <div>Агент : {{flat.relationships.attributes.first_name}} {{flat.relationships.attributes.last_name}} {{flat.relationships.attributes.middle_name}} id: {{flat.relationships.id}} </div>
                            <button class="btn waves-effect" @click.stop="addflatToViewed(flat)">Добавить в избранное</button>
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
                        
                        <div class="favorite-menu-item" v-for="flat in viewedflats" :key="flat.id">

                         <div>{{flat.attributes.title}}</div>
                          <div>Адресс : {{flat.attributes.address.city}}, {{flat.attributes.address.street}} {{flat.attributes.address.house}}, кв. {{flat.attributes.address.room}}</div>
                            <div>Агент : {{flat.relationships.attributes.first_name}} {{flat.relationships.attributes.last_name}} {{flat.relationships.attributes.middle_name}} id: {{flat.relationships.id}} </div>
                        <button class="btn btn-small" @click.stop="deleteflat(flat)">Удалить</button>
                        
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
               flats:[],
               showViewed:false,
               viewedflats:[]                   
           }
           },
           computed:{
               header(){               
                return this.request === "" || this.request === initheader ? "Ничего не ищем" : 'Идет поиск: '+ this.request;
               },
               filteredflats(){
                   if(this.request !== initheader){
                   return this.flats.filter( el => el.attributes.title.toLowerCase().includes(this.request.toLowerCase()) );
                   } else {
                       return this.flats;
                }

               }
               },

               methods: {
                addflatToViewed(flat){
                    if ( !this.viewedflats.find( el => el.id === flat.id) ){
                        this.viewedflats.push(flat);
                    }
                    this.showViewed = true;

                    setTimeout( () => {
                        this.showViewed = false;
                    }, 500)
                },
                deleteflat({id}){
                    this.viewedflats = this.viewedflats.filter( el => el.id !== id);
                }
            },
           
           async mounted(){
                this.flats = await fetch("https://raw.githubusercontent.com/FrolovArkadiy/task_for_middle/master/entities.json").then(res => res.json()).then(data => data.response);  
           }
        
        }
</script>


<style >

</style>
