<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="userRegistration">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Регистрация</h1>
          <label for="inputEmail" class="sr-only">Email</label>
          <input id="inputEmail" class="form-control" placeholder="Email" required="" v-model="email">
          <br>
          <label for="inputUsername" class="sr-only">Никнейм</label>
          <input id="inputUsername" class="form-control" placeholder="Никнейм" required="" v-model="username">
          <br>
          <label for="inputLastName" class="sr-only">Фамилия</label>
          <input id="inputLastName" class="form-control" placeholder="Фамилия"  v-model="last_name">
          <br>
          <label for="inputFirstName" class="sr-only">Имя</label>
          <input id="inputFirstName" class="form-control" placeholder="Имя"  v-model="first_name">
          <br>
          <label for="inputDate" class="sr-only">Дата рождения</label>
          <input  type="date" id="inputDate" class="form-control" placeholder="Дата рождения" v-model="date_of_birth">
          <br>
          <label for="inputPhone" class="sr-only">Телефон</label>
          <input  type="phone" id="inputPhone" class="form-control" placeholder="Телефон"  v-model="phone">
          <br>
          <label for="inputPhone">Выберите пол</label>
          <select class="form-control" name="Пол" id="inputSelect" placeholder="Пол"   v-model="sex">
            <option value="Male">Male</option>
            <option  value="Female">Female</option>
          </select>
          <br>
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" required="" v-model="password">
          <br>
          <label for="inputPassword" class="sr-only">Поавторите Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Повторите пароль" required="" v-model="password2">
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Зарегистрироваться</button>
          <a href = "/" class="btn mt-2 btn-lg btn-primary btn-block">Отмена</a>
        </form>
      </div>
    </div>
</template>
  
<script>
export default {
     layout: "post_detail",
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
            let response = await this.$axios.post('http://127.0.0.1:8000/api/v1/registration/', {
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

          this.$router.push('/signin')
        } catch (err) {
          console.log(err)
        }
      }
    }
  }
</script>
  
<style>
  
</style>