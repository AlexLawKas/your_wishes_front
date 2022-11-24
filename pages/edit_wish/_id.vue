<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="wishUpdate">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Редактировать желание</h1>
          <!-- <label for="inputEmail" class="sr-only">Email</label>
          <input id="inputEmail" value="profile.email" class="form-control" v-model="profile.email"> -->
          <br>
          <label for="inputName" class="sr-only">Название</label>
          <input type="text" id="inputName" ref="name"  class="form-control" placeholder="Название" required="" v-model="name">
          <br>
          <label for="inputDescription" class="sr-only">Описание</label>-
          <input id="inputDescription" value="" class="form-control" placeholder="Описание"  v-model="description">
          <br>
          <label for="inputUrl" class="sr-only">Ссылка</label>
          <input id="inputUrl" value="" class="form-control" placeholder="Ссылка"  v-model="url">
          <br>
          <label for="inputPrice" class="sr-only">Цена</label>
          <input  type="text" value="" id="inputPrice" class="form-control" placeholder="Цена" v-model="price">
          <br>
          <label for="inputDate" class="sr-only">Выполнить до:</label>
          <input  type="date" value="" id="inputDate" class="form-control" placeholder="Выполнить до:" v-model="deadline">
          <br>
          <label for="inputReason" class="sr-only">Повод</label>
          <input  type="text" value="" id="inputReason" class="form-control" placeholder="Повод"  v-model="reason">
          <br>
          <div class="large-12 medium-12 small-12 cell">
      <label>Изображение
        <input type="file" id="image" class="form-control" ref="image" v-on:change="handleFileUpload()"/>
      </label>
        </div>
        <div>Публичное или нет
        <input type="checkbox" id="checkbox" checked v-model="public"/>
<label for="checkbox"></label></div>
<div>Выполнено или нет
        <input type="checkbox" id="checkbox" checked v-model="done"/>
<label for="checkbox"></label></div>
          
          <!-- <br>
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" required="" v-model="password">
          <br>
          <label for="inputPassword" class="sr-only">Поавторите Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Повторите пароль" required="" v-model="password2"> -->
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Сохранить изменения</button>
          <a href = "/profile" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
<script>
import axios from "axios";

export default {
  data() 
    {
      return {
        wish: {},
        name:'',
        description:'',
        url:'',
        price:'',
       public:'',
       reason:'',
        done:'',
       deadline:''
      }
    },


  
      async fetch() {
        const wish_id = document.location.pathname.slice(6)

        const token = localStorage.getItem('auth._token.local')
        const config = {
          // 'Content-Type': 'application/json',
          'Content-Type': 'multipart/form-data',
          "Accept": "application/json",
          
          "Authorization": token
}
            this.wish = await fetch(`http://127.0.0.1:8000/api/v1/${wish_id}`, {withCredentials: false, headers: config}).then(res => res.json())

         
      
           this.name= this.wish.name,
           this.description=this.wish.description,
           this.url=this.wish.url,
           this.price=this.wish.price,
           this.public=this.wish.public,
           this.reason=this.wish.reason,
           this.done=this.wish.done,
           this.deadline=this.wish.deadline
          //  image:data.image,
},

fetchOnServer: true,
         
      
 
   

   
    
methods: {
  handleFileUpload(){
        this.image = this.$refs.image.files[0]
        


      },
 

      async wishUpdate() {
        try {
          const wish_id = document.location.pathname.slice(11)
          const { data } = await this.$axios.get(`http://127.0.0.1:8000/api/v1/wish/${wish_id}`, {
      })
     
       let formData = new FormData();
       if (this.image){
            formData.append('image', this.image)};
        formData.append('name', this.name);
        formData.append('description', this.description);
        if (this.last_name){
            formData.append('url', this.url)};
        if (this.price){
            formData.append('price', this.price)};
        if (this.deadline){
            formData.append('deadline', this.deadline)};
        if (this.reason){
            formData.append('reason', this.reason)};
        formData.append('public', this.public)
        formData.append('done', this.done)
            await this.$axios.put(`http://127.0.0.1:8000/api/v1/update_wish/${wish_id}`,
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
              }
            )
     
          this.$router.push(`/wish/${wish_id}`)
        } catch (err) {
          console.log(err)
        }
      },

      
    
    
  },
}
// var username = {{profile.username}};
// document.getElementById('inputUsername').innerHTML = username;

</script>
  
<style>
  
</style>