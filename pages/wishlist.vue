
<template>
    <div>
      <h1>Список желаний</h1>
      
      <br>
        <input v-model="search" class="form-control" placeholder="Поиск по названию">

        <br>
      <div v-for="wish in wishesByName" v-if="wish_list!=0" :key="wish.id" class="wish">
        <div class="wish_title"><nuxt-link class="nav-link" :to="`/wish/${wish.id}`">{{ wish.name }}</nuxt-link></div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>

        <div class="wish_image"> 
          <img :src=wish.image alt="" width="260" height="180"></div>
          <br>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
        <br>
        <br>
        <div class="wish_reason" style="color: red;"> {{ wish.deadline_of }}</div>
      </div>
      <div v-if="wish_list==0">
        <br>
        <p class="no_content">Список желаний пуст</p>
        
        </div> 
    </div>
      

    </template>
    
    <script>
    import axios from "axios";
    export default {
      data(){
    return {
      wish_list_status: NaN,
      wish_list : [],
      search: '',
      deadline_of: ''
    }
  },
      async fetch() {
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.wish_list = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/wish_list/`, {withCredentials: false, headers: config}).then(res => res.json().then(this.wish_list_status = res.status))
         if (this.wish_list_status == 401){ this.$router.push('/')};
         if (this.wish_list_status == 500){ this.$router.push('/500')};
         if (this.wish_list.length == 0) {
          this.wish_list = 0
         };
         for (let i = 0; i < this.wish_list.length; i += 1) {
      const wish = this.wish_list[i];
      
          if (wish.deadline_of == true){
            wish.deadline_of = 'Дедлай для выполнения желания уже прошел'
          }
        }

      //  const { data } = await axios.get(`http://127.0.0.1:8000/api/v1/wish_list/`);
    
         
      },
      computed: {
    wishesByName() {
      return this.wish_list.filter(item => item.name.indexOf(this.search) !== -1)
    },
  },
      fetchOnServer: true,
   
  methods: {
    searchWishes(){
      const { data } = this.$axios.get(`http://127.0.0.1:8000/api/v1/wish_list?name=`+this.q)

    }
  },
  
     
    }

   
    </script>
    
    <style>
    
    </style>