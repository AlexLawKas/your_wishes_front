<template>
    <div>
      <h1>Мой профиль</h1>
     <div class="main_profile">
      <div class="profile_nickname">Никнейм: {{ profile.username}}</div>
        <div class="profileh_title">Имя: {{ profile.first_name}}</div>
        <div class="profile_title">Фамилия: {{ profile.last_name}}</div>
        <div class="profile_title">Дата рождения: {{ profile.date_of_birth}}</div>
        <div class="profile_title">Пол: {{ profile.sex}}</div>
        <div class="profile_title">Телефон: {{ profile.phone}}</div>
        <div class="profile_title">Email: {{ profile.email}}</div>
        
        <div class="profile_image"> 
          <img :src=photo alt="Изображение" width="360" height="250">
        </div>
        <span><nuxt-link class="btn mt-2 btn-lg btn-primary" to="/edit_profile">Редактировать профиль</nuxt-link></span>
      
      </div>

  
        <h3>Мои желания</h3>
        <span><nuxt-link class="btn btn-outline-light button_add_wish" to="/create_wish">Добавить желаение</nuxt-link></span>
      <div v-for="wish in my_wishes" :key="wish.id" class="wish">
        <div class="wish_title">{{ wish.name }}</div>
        <div class="wish_description">{{ wish.description }}</div>
        
        <div class="wish_price">Цена: {{ wish.price }} руб</div>

        <div class="wish_image"> 
          <img :src=wish.image alt="Изображение"></div>
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
    const { data } = await axios.get(`http://127.0.0.1:8000/api/v1/profile/`, {withCredentials: false, headers: config});
    const wishes = await axios.get(`http://127.0.0.1:8000/api/v1/my_wishes/`, {withCredentials: false, headers: config});
    for (let i = 0; i < wishes.data.length; i += 1) {
    const wish = wishes.data[i];
    wish.image = "http://127.0.0.1:8000" + wish.image
    console.log(wish.image)

  // toLowerCase() — стандартный метод js,
  // преобразующий строку в нижний регистр
  // const normalizedEmail = email.toLowerCase();
  // Заменяем значение
  // emails[i] = normalizedEmail;
}

     return {
       profile: data,
       photo: "http://127.0.0.1:8000" + data.photo,

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
 
}


</script>

<style>