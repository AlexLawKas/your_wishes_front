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
          <img :src=wish.image alt="" width="200" height="120"></div>
        <br/>
        <div class="wish_reason">Повод: {{ wish.reason }}</div>
      </div>
      <div v-if="my_wishes==0">
        <br>
        <p class="no_content">Я еще не добавил ни одного желания</p>
        
        </div> 

      </div>
</template>



<script>
import axios from "axios";

export default {
      data(){
    return {
      profile_status:NaN,
      wishes_status:NaN,
      my_wishes : [],
      profile: {}, 
      photo: ''
      
    }
  },
      async fetch() {
        try{
       
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.my_wishes = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/my_wishes/`, {withCredentials: false, headers: config}).then(res => res.json().then(this.wishes_status = res.status))
         if (this.wishes_status == 401){ <div class="wish_price">Произошла ошибка</div>};
         if (this.wishes_status == 500){ <h2>произошла ошибка</h2>};
         if (this.my_wishes.length == 0) {
          this.my_wishes = 0
         };
         this.profile = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/profile/`, {withCredentials: false, headers: config}).then(res => res.json().then(this.profile_status = res.status))

         if (this.profile_status == 401){ this.$router.push('/')};
         if (this.profile_status == 500){ this.$router.push('/500')};
         if (this.wish_list_status == 403){ this.$router.push('/403')};
         

      for (let i = 0; i < this.my_wishes.length; i += 1) {
      const wish = this.my_wishes[i];
    wish.image = "http://localhost.charlesproxy.com:8000" + wish.image

    }
   this.photo= "http://localhost.charlesproxy.com:8000" + this.profile.photo
    
  }catch (err) {
          console.log(err)
          if (err.toString().includes('status code 500')) { this.$router.push('/500')};
        }},

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
