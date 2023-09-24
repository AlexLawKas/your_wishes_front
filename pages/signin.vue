<template>
    <div>
      <div class="text-center" style='overflow-y:hidden;'>
        <form class="form-signin" @submit.prevent="userLogin" style='overflow-y:hidden;'>
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Пожалуйста укажите Email и пароль</h1>
          <label for="inputEmail" class="sr-only">Email пользователя</label>
          <input id="inputEmail" class="form-control" placeholder="Email пользователя"  v-model="login.email">
          <p><span id="status_email"></span></p>
          <br>
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль"  v-model="login.password">
          <p><span id="status_password"></span></p>
          <br>
          <p><span id="status_auth"></span></p>
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Войти</button>
        </form>
      </div>
    </div>
</template>
  
<script>
  function isEmpty(str){
    return (str == null) || (str.length == 0)}
export default {
  
    // layout: "post_detail",
    data() {
      return {
        login: {
          email: '',
          password: '' 
        },
      }
    },
methods: {
      async userLogin() {
        try {
      const email_status = document.getElementById("status_email");
      const email = document.getElementById("inputEmail").value;
      if(isEmpty(email)) {email_status.innerHTML = '<span style="color:red;">' + "Поле Email пустое" + '</span>'
       return '';
        }
      const password_status = document.getElementById("status_password");
      const password = document.getElementById("inputPassword").value;
      if(isEmpty(password)) {password_status.innerHTML = '<span style="color:red;">' + "Поле Пароль пустое" + '</span>'
       return '';
        }
          let response = await this.$auth.loginWith('local', { data: this.login })
          console.log(response)

          this.$router.push('/')
        } catch (err) {
          const auth_status = document.getElementById("status_auth");
          if (err.toString().includes('status code 401')) {auth_status.innerHTML = '<span style="color:red;">' + "Неверный Email или пароль" + '</span>'
       return '';
        }
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