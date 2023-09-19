<template>
    <div>
      <div class="text-center" style='overflow-y:hidden;'>
        <form class="form-signin" @submit.prevent="userLogin" style='overflow-y:hidden;'>
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Пожалуйста укажите Email и пароль</h1>
          <label for="inputEmail" class="sr-only">Email пользователя</label>
          <input id="inputEmail" class="form-control" placeholder="Email пользователя" required="" v-model="login.email">
          <br>
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" required="" v-model="login.password">
          <br>
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Войти</button>
        </form>
      </div>
    </div>
</template>
  
<script>
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
          let response = await this.$auth.loginWith('local', { data: this.login })
          console.log(response)

          this.$router.push('/')
        } catch (err) {
          if (err.toString().includes('status code 500')) { this.$router.push('/500')};
          console.log(err)
        }
      }
    }
  }
</script>
  
<style>
  
</style>