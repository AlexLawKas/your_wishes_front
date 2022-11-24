<template>
    <div>
      <h1>Профиль пользователя {{ user_profile.username}}</h1>
     <div class="main_profile" >
      <div class="profile_nickname">Никнейм: {{ user_profile.username}}</div>
        <div class="profileh_title">Имя: {{ user_profile.first_name}}</div>
        <div class="profile_title">Фамилия: {{ user_profile.last_name}}</div>
        <div class="profile_title">Дата рождения: {{ user_profile.date_of_birth}}</div>
        <div class="profile_title">Пол: {{ user_profile.sex}}</div>
        <div class="profile_title">Телефон: {{ user_profile.phone}}</div>
        <div class="profile_title">Email: {{ user_profile.email}}</div>
        
        <div class="profile_image"> 
          <img :src=photo alt="Изображение" width="360" height="250">
        </div>
        <!-- <span><nuxt-link class="btn mt-2 btn-lg btn-primary" to="/edit_profile">Редактировать профиль</nuxt-link></span> -->
      
      </div>

  
        <h3>Желания {{ user_profile.username}}</h3>
      <div v-for="wish in my_wishes" :key="wish.id" class="wish">
        <div class="wish_title"><nuxt-link class="nav-link" :to="`/wish/${wish.id}`">{{ wish.name }}</nuxt-link></div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>

        <div class="wish_image"> 
          <img :src=wish.image alt="" width="260" height="180"></div>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
      </div>

      </div>
    
  
</template>



<script>
import axios from "axios";

export default {
  async asyncData({params, $http }) {
    
    
     const token = localStorage.getItem('auth._token.local')
     const config = {
       'Content-Type': 'application/json',
       "Accept": "application/json",
           "Authorization": token}

 	

 const  user_profile  = await axios.get(`http://127.0.0.1:8000/api/v1/user_detail/${params.id}`, {withCredentials: false, headers: config});

    const wishes = await axios.get(`http://127.0.0.1:8000/api/v1/wish_list?created_by=${params.id}`, {withCredentials: false, headers: config});

    if (wishes.data == []){
        wishes.data = 'Список желаний пуст'
    }
     return {
        user_profile: user_profile.data,
        photo: "http://127.0.0.1:8000" + user_profile.data.photo,
       my_wishes: wishes.data,
       
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
computed:  {
  loggedIn() {
      return this.$auth.loggedIn
    },
  
     
    
  },
}

  
</script>

<style>