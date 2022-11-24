<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="createWish">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Создать желание</h1>
          <br>
          <label for="inputName" class="sr-only">Название</label>
          <input type="text" id="inputUsername" ref="username"  class="form-control" placeholder="Название" required="" v-model="name">
          <br>
          <label for="inputDescription" class="sr-only">Описание</label>
          <input id="inputDescription" value="" class="form-control" placeholder="Описание" required=""  v-model="description">
          <br>
          <label for="inputPrice" class="sr-only">Цена</label>
          <input  type="int"  id="inputPrice" class="form-control" placeholder="Цена" required="" v-model="price">
          <br>
          <label for="inputUrl" class="sr-only">Ссылка</label>
          <input  type="url" value="" id="inputUrl" class="form-control" placeholder="Ссылка"  v-model="url">
          <br>
          <label for="inputReason" class="sr-only">Повод</label>
          <input  type="text" value="" id="inputReason" class="form-control" placeholder="Повод"  v-model="reason">
          <br>
          <label for="inputDeadline" class="sr-only">Срок выполнения</label>
          <input  type="date" value="" id="inputDeadline" class="form-control" placeholder="Срок выполнения" v-model="deadline">
          <br>
          <div class="large-12 medium-12 small-12 cell">
      <label>Изображение
        <input type="file" id="image" class="form-control" ref="image" v-on:change="handleFileUpload()"/>
      </label>
        </div>
        <div>Публичное или нет
        <input type="checkbox" id="checkbox" checked v-model="public"/>
<label for="checkbox"></label>
</div>
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Создать</button>
          <a href = "/profile" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
  
<script>
export default {
    layout: "post_detail",
    data() {
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
      
      async createWish() {
        let formData = new FormData();
       if (this.image){
            formData.append('image', this.image)};
        formData.append('description', this.description);
        formData.append('name', this.name);
      if (this.public == undefined){
        this.public =true
      };
      formData.append('public', this.public)

        if (this.url){
            formData.append('url', this.url)};
        if (this.price){
            formData.append('price', this.price)};
        if (this.deadline){
            formData.append('deadline', this.deadline)};
        if (this.reason){
            formData.append('reason', this.reason)};

        try {
            await this.$axios.post('http://127.0.0.1:8000/api/v1/wish/',
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
        }
      }
    }
  }
</script>
  
<style>
  
</style>