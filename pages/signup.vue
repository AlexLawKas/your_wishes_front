<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="userRegistration">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Регистрация</h1>
          <label for="inputEmail" class="sr-only">Email</label>
          <input id="inputEmail" class="form-control" placeholder="Email"  v-model="email">
          <p><span id="status_email"></span></p>
          <br>
          <label for="inputUsername" class="sr-only">Никнейм</label>
          <input id="inputUsername" class="form-control" placeholder="Никнейм"  v-model="username">
          <p><span id="status_username"></span></p>
          <br>
          <label for="inputLastName" class="sr-only">Фамилия</label>
          <input id="inputLastName" class="form-control" placeholder="Фамилия"  v-model="last_name">
          <p><span id="last_name_status"></span></p>
          <br>
          <label for="inputFirstName" class="sr-only">Имя</label>
          <input id="inputFirstName" class="form-control" placeholder="Имя"  v-model="first_name">
          <p><span id="first_name_status"></span></p>
          <br>
          <label for="inputDate" class="sr-only">Дата рождения</label>
          <input  type="date" id="inputDate" class="form-control" placeholder="Дата рождения" v-model="date_of_birth">
          <p><span id="date_status"></span></p>
          <br>
          <label for="inputPhone" class="sr-only">Телефон</label>
          <input  type="phone" id="inputPhone" class="form-control" placeholder="Телефон" onkeypress="if ( isNaN( String.fromCharCode(event.keyCode) )) return false;"
 v-model="phone">
          <p><span id="phone_status"></span></p>
          <br>
          <label for="inputSelect">Выберите пол</label>
          <select class="form-control" name="Пол" id="inputSelect" placeholder="Пол"   v-model="sex">
            <option value="Male">Male</option>
            <option  value="Female">Female</option>
          </select>
          <br>
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль"  v-model="password">
          <p><span id="status_password"></span></p>
          <br>
          <label for="inputRepeatPassword" class="sr-only">Поавторите Пароль</label>
          <input type="password" id="inputRepeatPassword" class="form-control mt-2" placeholder="Повторите пароль"  v-model="password2">
          <p><span id="status_repeat_password"></span></p>
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Зарегистрироваться</button>
          <a href = "/" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
<script>
function isEmpty(str){
    return (str == null) || (str.length == 0)}
export default {
    data() 
    {
      return {
        registration: {
          email: '',
          username:'',
          last_name:'',
          first_name:'',
          date_of_birth:'',
          phone:'',
          sex:'',
          password: '',
          password2: ''
        },
      }
    },
    
methods: {
      async userRegistration() {
        try {

          const email_status = document.getElementById("status_email");
      const email = document.getElementById("inputEmail").value;
      if(isEmpty(email)) {email_status.innerHTML = '<span style="color:red;">' + "Поле Email пустое" + '</span>'
       return '';
        }
      const  re = /^[^\s()<>@,;:\/]+@\w[\w\.-]+\.[a-z]{2,}$/i;
      if (!re.test(email)){email_status.innerHTML = '<span style="color:red;">' + "Вы ввели некорректный Email" + '</span>';
      return 'Стоп';}
      if(email.length >60) {email_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 60 символов" + '</span>';
      return 'Стоп';}
      else{ email_status.innerHTML = '' }
       
      const username_status = document.getElementById("status_username");
      const username = document.getElementById("inputUsername").value;
      if(isEmpty(username)) {username_status.innerHTML = '<span style="color:red;">' + "Поле Никнейм пустое" + '</span>';
      return 'Стоп';}
      if(username.length >50) {username_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 50 символов" + '</span>';
      return 'Стоп';}
      else{ username_status.innerHTML = '' }

      const password_status = document.getElementById("status_password");
      const password = document.getElementById("inputPassword").value;
      if(isEmpty(password)) {password_status.innerHTML = '<span style="color:red;">' + "Поле Пароль пустое" + '</span>';
      return 'Стоп';}
      else{ password_status.innerHTML = '' }

      const repeat_password_status = document.getElementById("status_repeat_password");
      const repeat_password = document.getElementById("inputRepeatPassword").value;
      if(isEmpty(repeat_password)) {repeat_password_status.innerHTML = '<span style="color:red;">' + "Поле Повторить пароль пустое" + '</span>';
      return 'Стоп';}
      else{ repeat_password_status.innerHTML = '' }
      if (password != repeat_password)  {repeat_password_status.innerHTML = '<span style="color:red;">' + "Пароли не совпадают" + '</span>';
      return 'Стоп';}
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
      if((phone.length >12) || ((phone.length >= 1) && (phone.length < 9))) {phone_status.innerHTML = '<span style="color:red;">' + "Длина поля должна быть не более 12 символов ии не менее 9 символов" + '</span>';
      return 'Стоп';}
      else{ phone_status.innerHTML = '' }
      const date_status = document.getElementById("date_status");
      const date = document.getElementById("inputDate").value;
      if(new Date(date) > new Date()) {date_status.innerHTML = '<span style="color:red;">' + "Дата не может быть в будущем" + '</span>';
      return 'Стоп';}
      else{ date_status.innerHTML = '' }

            let response = await this.$axios.post('http://localhost.charlesproxy.com:8000/api/v1/registration/', {
        email: this.email,
        username: this.username,
        last_name: this.last_name,
        first_name: this.first_name,
        date_of_birth: this.date_of_birth,
        phone: this.phone,
        sex: this.sex,
        password: this.password,
        password2: this.password2,
      })
      console.log(response)
          console.log(response.data.access)  
          console.log(response.status)
          if ('status code 500' in err){this.$router.push('/500.html')}


          this.$router.push('/signin')
          
        } catch (err) {
          console.log(err)
          if (err.toString().includes('status code 500')) { this.$router.push('/500')};
        }
      },
     
  }
}
</script>
  
<style>
</style>