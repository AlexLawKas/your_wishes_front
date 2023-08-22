<template>
     <div>
     <h1>Мой профиль</h1>
     <div class="main_profile" >
      <div class="profile_nickname">Никнейм: {{ profile.username}}</div>
      
        <div class="profileh_title">Имя: {{ profile.first_name}}</div>
        <div class="profile_title">Фамилия: {{ profile.last_name}}</div>
        <div class="profile_title">Дата рождения: {{ profile.date_of_birth}}</div>
        <div class="profile_title">Пол: {{ profile.sex}}</div>
        <div class="profile_title">Телефон: {{ profile.phone}}</div>
        <div class="profile_title">Email: {{ profile.email}}</div>
        
        <div class="profile_image"> 
          <img :src=photo alt="Изображение" width="250" height="300">
        </div>
        <span><nuxt-link class="btn mt-2 btn-lg btn-primary" to="/edit_profile">Редактировать профиль</nuxt-link></span>
      
      </div>

  
        <h3>Мои желания</h3>
        <span><nuxt-link class="btn btn-outline-light button_add_wish" to="/create_wish">Добавить желаение</nuxt-link></span>
      <div v-for="wish in my_wishes" :key="wish.id" class="wish">
        <div class="wish_title"><nuxt-link class="nav-link" :to="`/wish/${wish.id}`">{{ wish.name }}</nuxt-link></div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>
        <br/>
        <div class="wish_image"> 
          <img :src=wish.image alt="" width="230" height="180"></div>
        <br/>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
      </div>

      </div>
    

</template>



<script>
import axios from "axios";

export default {
      data(){
    return {
      my_wishes : [],
      profile: {}, 
      photo: ''
      
    }
  },
      async fetch() {
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.my_wishes = await fetch(`http://127.0.0.1:8000/api/v1/my_wishes/`, {withCredentials: false, headers: config}).then(res => res.json())
         this.profile = await fetch(`http://127.0.0.1:8000/api/v1/profile/`, {withCredentials: false, headers: config}).then(res => res.json())
         

      for (let i = 0; i < this.my_wishes.length; i += 1) {
      const wish = this.my_wishes[i];
    wish.image = "http://127.0.0.1:8000" + wish.image

    }
   this.photo= "http://127.0.0.1:8000" + this.profile.photo
    
  },

      fetchOnServer: true,

 method:{
  


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
