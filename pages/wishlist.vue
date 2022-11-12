
<template>
    <div>
      <h1>Список желаний</h1>
      <form class="form-inline my-2 my-lg-0">
        <input name="q" v-model="q" type="text" class="form-control mr-sm-2" placeholder="Поиск" aria-label="Поиск">
        <button class="btn btn-outline-success my-2 my-sm-0 mr-2" type="submit" @click.stop.prevent="submit()">Поиск</button>
      </form>
  
      <div v-for="wish in wish_list" :key="wish.id" class="wish">
        <div class="wish_title"><nuxt-link class="nav-link" to="/wish">{{ wish.name }}</nuxt-link></div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>

        <div class="wish_image"> 
          <img :src=wish.image alt="Изображение"  width="260" height="150"></div>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
      </div>
    </div>
      

    </template>
    
    <script>
    import axios from "axios";
    export default {
      async asyncData(ctx) {
        const token = localStorage.getItem('auth._token.local')
        const config = {
          'Content-Type': 'application/json',
          "Accept": "application/json",
          "Authorization": token
}
        const { data } = await axios.get(`http://127.0.0.1:8000/api/v1/wish_list/`, {withCredentials: false, headers: config});

         return {
           wish_list: data,
         }
         
      },
      data(){
    return {
      q : null,
    }
  },
  methods: {
    submit(){
      this.$router.push("http://127.0.0.1:8000/api/v1/wish_list?name="+this.q);
    }
  },
     
    }

   
    </script>
    
    <style>
    
    </style>