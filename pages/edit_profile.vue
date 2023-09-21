<template>
    <div>
      <div class="text-center">
        <form class="form-signup" @submit.prevent="userUpdate">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Редактировать профиль</h1>
          <!-- <label for="inputEmail" class="sr-only">Email</label>
          <input id="inputEmail" value="profile.email" class="form-control" v-model="profile.email"> -->
          <br>
          <label for="inputUsername" class="sr-only">Никнейм</label>
          <input type="text" id="inputUsername" ref="username"  class="form-control" placeholder="Никнейм"  v-model="username">
          <p><span id="status_username"></span></p>
          <br>
          <label for="inputLastName" class="sr-only">Фамилия</label>-
          <input id="inputLastName" value="" class="form-control" placeholder="Фамилия"  v-model="last_name">
          <p><span id="last_name_status"></span></p>
          <br>
          <label for="inputFirstName" class="sr-only">Имя</label>
          <input id="inputFirstName" value="" class="form-control" placeholder="Имя"  v-model="first_name">
          <p><span id="first_name_status"></span></p>
          <br>
          <label for="inputDate" class="sr-only">Дата рождения</label>
          <input  type="date" value="" id="inputDate" class="form-control" placeholder="Дата рождения" v-model="date_of_birth">
          <p><span id="date_status"></span></p>
          <br>
          <label for="inputPhone" class="sr-only">Телефон</label>
          <input  type="phone" value="" id="inputPhone" class="form-control" placeholder="Телефон"  v-model="phone">
          <p><span id="phone_status"></span></p>
          <br>
          <label for="inputSelect">Выберите пол</label>
          <select value="" class="form-control" name="Пол" id="inputSelect" placeholder="Пол"   v-model="sex">
            <option value="Male">Муж.</option>
            <option  value="Female">Жен.</option>
          </select>
          <br>
          <div class="large-12 medium-12 small-12 cell">
      <label>Аватар
        <input type="file" id="photo" class="form-control" ref="photo" v-on:change="handleFileUpload()"/>
      </label>
      
    </div>
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Сохранить изменения</button>
          <a href = "/profile" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
<script>
function isEmpty(str){
    return (str == null) || (str.length == 0)}
import axios from "axios";
export default {

  
    data(){
   
  
      return {
         profile: {

         },
         username:'',
         last_name:'',
         first_name:'',
         date_of_birth:'',
         phone:'',
         sex:''
 
      
    
  }
},
      async fetch() {
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.profile = await fetch(`http://localhost.charlesproxy.com:8000/api/v1/profile/`, {withCredentials: false, headers: config}).then(res => res.json())
      
       
          this.username = this.profile.username,
          this.last_name = this.profile.last_name,
          this.first_name=this.profile.first_name,
          this.date_of_birth=this.profile.date_of_birth,
          this.phone=this.profile.phone,
          this.sex=this.profile.sex

  },

      fetchOnServer: true,
    
methods: {
  handleFileUpload(){
        this.photo = this.$refs.photo.files[0]
      },
 
      async userUpdate() {
        try {
          const username_status = document.getElementById("status_username");
      const username = document.getElementById("inputUsername").value;
      if(isEmpty(username)) {username_status.innerHTML = '<span style="color:red;">' + "Поле Никнейм пустое" + '</span>';
      return 'Стоп';}
      if(username.length >50) {username_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ username_status.innerHTML = '' }

      
      const first_name_status = document.getElementById("first_name_status");
      const first_name = document.getElementById("inputFirstName").value;
      if(first_name.length >49) {first_name_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ first_name_status.innerHTML = '' }

      const last_name_status = document.getElementById("last_name_status");
      const last_name = document.getElementById("inputLastName").value;
      if(last_name.length >50) {last_name_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ last_name_status.innerHTML = '' }

      const phone_status = document.getElementById("phone_status");
      const phone = document.getElementById("inputPhone").value;
      console.log('Длина телефона',phone.length)
      if((phone.length >12) || ((phone.length >= 1) && (phone.length < 9))) {phone_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 12 символов и не менее 9 символов" + '</span>';
      return 'Стоп';}
      else{ phone_status.innerHTML = '' }   

      const date_status = document.getElementById("date_status");
      const date = document.getElementById("inputDate").value;
      if(new Date(date) > new Date()) {date_status.innerHTML = '<span style="color:red;">' + "Дата не может быть в будущем" + '</span>';
      return 'Стоп';}
      else{ date_status.innerHTML = '' } 
          const { data } = await this.$axios.get(`http://localhost.charlesproxy.com:8000/api/v1/profile/`, {
      })
     
       let formData = new FormData();
       if (this.photo){
            formData.append('photo', this.photo)};
        formData.append('email', data.email);
        formData.append('username', this.username);
        if (this.last_name){
            formData.append('last_name', this.last_name)};
        if (this.first_name){
            formData.append('first_name', this.first_name)};
        if (this.date_of_birth){
            formData.append('date_of_birth', this.date_of_birth)};
        if (this.phone){
            formData.append('phone', this.phone)};
        if (this.sex) {
             
            formData.append('sex', this.sex)};
            await this.$axios.put('http://localhost.charlesproxy.com:8000/api/v1/user_update/',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
              }
            )
    
          this.$router.push('/profile')
        } catch (err) {
          console.log(err)
          if (err.toString().includes('status code 500')) { this.$router.push('/500')};
        }
      }
    },
      
  }


</script>
  
<style>
  
</style>