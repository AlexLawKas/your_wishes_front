<template>
    <div>
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
    async asyncData({params}) {
      const token = localStorage.getItem('auth._token.local')
      const config = {
       'Content-Type': 'application/json',
       "Accept": "application/json",
       "Authorization": token
 }
 
      const wish = await axios.get(`http://127.0.0.1:8000/api/v1/wish/${params.id}`,  {withCredentials: false, headers: config});
      
      const  user_profile  = await axios.get(`http://127.0.0.1:8000/api/v1/user_detail/${wish.data.created_by}`, {withCredentials: false, headers: config});
     
      const  profile  = await axios.get(`http://127.0.0.1:8000/api/v1/profile`, {withCredentials: false, headers: config});
      
      wish.data.image = "http://127.0.0.1:8000" + wish.data.image
      if (wish.data.done == false){
        wish.data.done = 'Желание еще не выполнено'
      }
      if (wish.data.done == true){
        wish.data.done = 'Желание уже выполнено'
      }
      if (wish.data.url == null){
        wish.data.url = '-'
      }
      if (wish.data.reason == null){
        wish.data.reason = '-'
      }
      if (wish.data.deadline == null){
        wish.data.deadline = '-'
      }
      console.log(user_profile.data.id)
      wish.data.created_by = user_profile.data.username
      console.log(profile.data.username)
      console.log( wish.data.created_by)

      return {
        wish: wish.data,
        user_profile: user_profile.data,
        profile: profile.data
      }
    },
    data(){
return {
  q : null,
}
},
methods:{
deleteWish() {

   const is_delete = confirm("Вы уверны что хотите удалить желание?");
   if  (is_delete){
    const wish_id = document.location.pathname.slice(6)
        try {
          this.$axios.delete(`http://127.0.0.1:8000/api/v1/delete_wish/${wish_id}`, {
      })
      
    } catch (err) {
          console.log(err)
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