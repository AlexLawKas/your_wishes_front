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
          <a href = "/" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
<script>
import axios from "axios";
const wish_id = document.location.pathname.slice(6)
export default {


  
      async asyncData(ctx) {

        const token = localStorage.getItem('auth._token.local')
        const config = {
          // 'Content-Type': 'application/json',
          'Content-Type': 'multipart/form-data',
          "Accept": "application/json",
          
          "Authorization": token
}
            const { data } = await axios.get(`http://127.0.0.1:8000/api/v1/wish/${wish_id}`, {withCredentials: false, headers: config});

         return {
           wish: data,
           name: data.name,
           description:data.description,
           url:data.url,
           price:data.price,
           public:data.public,
           reason:data.reason,
           done:data.done,
           deadline:data.deadline,
          //  image:data.image,

         }
         
         
      },
   

    data() 
    {
      return {
        wish: {
          name: '',
          description: '',
          image: '',
          url:'',
          price:'',
          public: '',
          reason:'',
          deadline:'',
          done:''
        },
      }
    },
    
methods: {
  handleFileUpload(){
        this.image = this.$refs.image.files[0]
        


      },
 

      async wishUpdate() {
        try {
          const { data } = await this.$axios.get(`http://127.0.0.1:8000/api/v1/wish/${wish_id}`, {
      })
     

     
       let formData = new FormData();
       if (this.image){
            formData.append('image', this.image)};
        formData.append('name', data.name);
        formData.append('description', this.description);
        if (this.last_name){
            formData.append('url', this.url)};
        if (this.price){
            formData.append('price', this.price)};
        if (this.deadline){
            formData.append('deadline', this.deadline)};
        if (this.rerason){
            formData.append('rerason', this.rerason)};
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