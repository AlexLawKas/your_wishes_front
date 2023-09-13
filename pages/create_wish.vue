<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="createWish">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Создать желание</h1>
          <br>
          <label for="inputName" class="sr-only">Название</label>
          <input type="text" id="inputName" ref="name"  class="form-control" placeholder="Название" v-model="name">
          <p><span id="status_name"></span></p>
          <br>
          <label for="inputDescription" class="sr-only">Описание</label>
          <input id="inputDescription" value="" class="form-control" placeholder="Описание"   v-model="description">
          <p><span id="status_description"></span></p>
          <br>
          <input  type="int"  id="inputPrice" class="form-control" placeholder="Цена"  v-model="price">
          <p><span id="status_price"></span></p>
          <br>
          <label for="inputUrl" class="sr-only">Ссылка</label>
          <input  type="url" value="" id="inputUrl" class="form-control" placeholder="Ссылка"  v-model="url">
          <p><span id="status_url"></span></p>
          <br>
          <label for="inputReason" class="sr-only">Повод</label>
          <input  type="text" value="" id="inputReason" class="form-control" placeholder="Повод"  v-model="reason">
          <p><span id="status_reason"></span></p>
          <br>
          <label for="inputDeadline" class="sr-only">Срок выполнения</label>
          <input  type="date" value="" id="inputDeadline" class="form-control" placeholder="Срок выполнения" v-model="deadline">
          <p><span id="status_deadline"></span></p>
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
function isEmpty(str){
    return (str == null) || (str.length == 0)}
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
        const status_name = document.getElementById("status_name");
      const name = document.getElementById("inputName").value;
      if(isEmpty(name)) {status_name.innerHTML = '<span style="color:red;">' + "Поле Название пустое" + '</span>'
       return '';
        }
        if(name.length >50) {status_name.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ status_name.innerHTML = '' }

      const status_description = document.getElementById("status_description");
      const description = document.getElementById("inputDescription").value;
      if(isEmpty(description)) {status_description.innerHTML = '<span style="color:red;">' + "Поле Описание пустое" + '</span>'
       return '';
        }
        if(description.length >50) {status_description.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 250 символов" + '</span>';
      return 'Стоп';}
      else{ status_description.innerHTML = '' }

      const status_price = document.getElementById("status_price");
      const price = document.getElementById("inputPrice").value;
      if(isEmpty(price)) {status_price.innerHTML = '<span style="color:red;">' + "Поле Цена пустое" + '</span>'
       return '';
        }
        if(price.length >10) {status_price.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 10 символов" + '</span>';
      return 'Стоп';}
      if(price == 0) {status_price.innerHTML = '<span style="color:red;">' + "Цена не может быть равна 0" + '</span>';
      return 'Стоп';}
      if(price<0) {status_price.innerHTML = '<span style="color:red;">' + "Цена не может быть отрицательной" + '</span>';
      return 'Стоп';}
      else{ status_price.innerHTML = '' }

      const status_reason = document.getElementById("status_reason");
      const reason = document.getElementById("inputReason").value;
        if(reason.length >50) {status_reason.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ status_reason.innerHTML = '' }

      const status_deadline = document.getElementById("status_deadline");
      const deadline = document.getElementById("inputDeadline").value;
      if(new Date(deadline) < new Date()) {status_deadline.innerHTML = '<span style="color:red;">' + "Срок выполнения не может быть в прошолом" + '</span>';
      return 'Стоп';}
      else{ status_deadline.innerHTML = '' }



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
          let response = await this.$axios.post('http://localhost.charlesproxy.com:8000/api/v1/wish/',
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
          console.log(err)
        }
      }
    }
  }
</script>
  
<style>
  
</style>