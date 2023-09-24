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
   

      <div v-for="wish in wishes" :key="wish.id" class="wish">
        <div class="wish_title"><nuxt-link class="nav-link" :to="`/wish/${wish.id}`">{{ wish.name }}</nuxt-link></div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>

        <div class="wish_image"> 
          <img :src=wish.image alt="" width="200" height="120"></div>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
      </div>
  
    
        <div v-if="wishes==0">
          <br>
          <p class="no_content">Пользователь {{ user_profile.username}} еще не добавил ни одного желания</p>
          
          </div> 
      
      </div>
    
  
</template>



<script>
import axios from "axios";

export default {
  data(){
    return {
      users_status: NaN,
      user_profile : [],
     
      photo: ''
      
    }
  },
      async fetch() {
        const user_id = document.location.pathname.slice(13)
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.user_profile = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/user_detail/${user_id}`, {withCredentials: false, headers: config}).then(res => res.json().then(this.users_status = res.status))
         if (this.user_profile.sex == 'Male'){this.user_profile.sex = 'Мужской'}
      if (this.user_profile.sex == 'Female'){this.user_profile.sex = 'Женский'}
         if (this.users_status == 404){ this.$router.push('/404')};
         if (this.users_status == 401){ this.$router.push('/')};
         if (this.users_status == 500){ this.$router.push('/500')};
         if (this.users_status == 403){ this.$router.push('/403')};
         this.wishes = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/wish_list?created_by=${user_id}`, {withCredentials: false, headers: config}).then(res => res.json())
         if (this.wishes.length == 0) {
          this.wishes = 0
         }
   this.photo= "http://localhost.charlesproxy.com:8000" + this.user_profile.photo
    
  },

      fetchOnServer: true,

methods: {


submit(){
  this.$router.push("http://localhost.charlesproxy.com:8000/api/v1/wish_list?name="+this.q);
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