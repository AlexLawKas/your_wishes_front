<template>
    <div class="item">
      <h1>{{ wish.name }}</h1>
     <div class="main_profile">
        <div class="wish_author">Автор: <nuxt-link class="nav-link" :to="`/user_detail/${user_profile.id}`">{{ wish.created_by }}</nuxt-link></div>
        <div class="wish_description">Описание: {{ wish.description }}</div>
        <div class="wish_price">Цена: {{ wish.price}} руб</div>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
        <div class="wish_url">Ссылка: {{ wish.url }}</div>
        <div class="wish_deadline">Выполнить до: {{ wish.deadline}}</div>
        <div class="wish_created_at">Дата создания: {{ wish.created_at}}</div>
        <div class="wish_done">{{ wish.done}}</div>
        
        <div class="wish_image_in_wish_page"> 
          <img :src=wish.image alt="Изображение" width="360" height="250">
        </div>
        <span v-if="wish.created_by == profile.username" ><nuxt-link class="btn mt-2 btn-lg btn-primary" :to="`/edit_wish/${wish.id}`">Редактировать желание</nuxt-link>
          <button class="btn mt-2 btn-lg btn-primary delete"  @click.prevent="deleteWish">Удалить желание </button>
        </span>
      </div>

      </div>
    
  
</template>
<div id="result"></div>
  <script>
  import axios from "axios";
  
  export default 
  {
    data(){
    return {
      wish_status : NaN,
      wish : {},
      user_profile:{},
      profile: {}, 
      
    }
  },
      async fetch() {
        const wish_id = document.location.pathname.slice(6)
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}
          
         this.wish = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/wish/${wish_id}`,  {withCredentials: false, headers: config}).then(res => res.json().then(this.wish_status = res.status))
         
         if (this.wish_status == 404){ this.$router.push('/404')};
         if (this.wish_status == 401){ this.$router.push('/')};
         if (this.wish_status == 500){ this.$router.push('/500')};
         if (this.wish_status == 403){ this.$router.push('/403')};
         this.user_profile = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/user_detail/${this.wish.created_by}`, {withCredentials: false, headers: config}).then(res => res.json())
         this.profile = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/profile/`, {withCredentials: false, headers: config}).then(res => res.json())

         this.wish.image = "http://localhost.charlesproxy.com:8000" + this.wish.image
         this.name = this.wish.name,
    this.description = this.wish.description,
    this.url=this.wish.url,
    this.price=this.wish.price,
    this.public=this.wish.public,
    this.reason=this.wish.reason,
    this.done=this.wish.done,
    this.deadline=this.wish.deadline,
    this.wish.created_by = this.user_profile.username
      if (this.wish.done == false){
        this.wish.done = 'Желание еще не выполнено'
      }
      if (this.wish.done == true){
        this.wish.done = 'Желание уже выполнено'
      }
      if (this.wish.url == null){
        this.wish.url = '-'
      }
      if (this.wish.reason == null){
        this.wish.reason = '-'
      }
      if (this.wish.deadline == null){
        this.wish.deadline = '-'
      }

  },

      fetchOnServer: true,

methods:{
deleteWish() {

   const is_delete = confirm("Вы уверны что хотите удалить желание?");
   if  (is_delete){
    const wish_id = document.location.pathname.slice(6)
        try {
          this.$axios.delete(`http://localhost.charlesproxy.com:8000/api/v1/delete_wish/${wish_id}`, {
      })
      
    } catch (err) {
          console.log(err)
          if (err.toString().includes('status code 500')) { this.$router.push('/500')};
          if (err.toString().includes('status code 404')) { this.$router.push('/404')};
          if (err.toString().includes('status code 401')) { this.$router.push('/')};
          if (err.toString().includes('status code 403')) { this.$router.push('/403')};
        }
    this.$router.push(`/profile`)
   }
   else {this.$router.push(`/profile`)}

    },

  },
}
  </script>
  
  <style scoped>
  
  </style>