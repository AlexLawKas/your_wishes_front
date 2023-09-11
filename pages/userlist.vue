
<template>
    <div>
      <h1>Пользователи</h1>
      <br>
      <input v-model="search" class="form-control" placeholder="Поиск по никнейму">
       <br>
      <table cellpadding="7" border="2" width="100%">
        <tr>
         <th>Никнейм</th>
         <th>Фамилия</th>
         <th>Имя</th>
         <th>Дата рождения</th>
         <th>Пол</th>
         <th>Последняя активность</th>
        </tr>
       
        <tbody>
          <tr> {{notes}}</tr>
          <tr v-for="user in usersByNikc " :key="user.id" class="user1">
            <td class="wish_author"><nuxt-link class="nav-link" :to="`/user_detail/${user.id}`">{{ user.username}}</nuxt-link></td>
            <td class="profile_title">  {{ user.first_name}}  </td>
            <td class="profile_title">  {{ user.last_name}}  </td>
            <td class="profile_title">  {{ user.date_of_birth}}  </td>
            <td class="profile_title">  {{ user.sex}}  </td>
            <td class="profile_title">  {{ user.lats_active}}  </td>
          </tr>
        </tbody>
     
         
        </tr>
        
       </table>
       
  
  
    </div>
      

    </template>
    
    <script>
    import axios from "axios";
    export default {
      data(){
        
    return {
      user_list_status: NaN,
      user_list : [],
      search: '',
      notes: ''
      
    }
  },
  
      async fetch() {
         const token = localStorage.getItem('auth._token.local')
         const config = {
           'Content-Type': 'application/json',
           "Accept": "application/json",
           "Authorization": token}

         this.user_list = await fetch(`http://127.0.0.1:8000/api/v1/user_list/`, {withCredentials: false, headers: config}).then(res => res.json().then(this.user_list_status = res.status))
         if (this.user_list_status == 401){ this.$router.push('/')};
         if (this.user_list_status == 500){ this.$router.push('/500')};
         if (this.user_list.length == 0){
            this.notes = 'Результаты не найдены'
         }
         for (let i = 0; i < this.user_list.length; i += 1) {
      const user = this.user_list[i];
      
          if (user.sex == 'Male'){
            user.sex = 'Муж.'
          }
          if (user.sex == 'Female'){
             user.sex = 'Жен.'
          }
         }

    
         
      },
      computed: {
    usersByNikc() {
      const users = this.user_list.filter(item => item.username.indexOf(this.search) !== -1)
      if( users.length > 0){
        return  this.user_list.filter(item => item.username.indexOf(this.search) !== -1)
      }
      
    },
  },
      fetchOnServer: true,
   
  methods: {
    submit(){
      this.$router.push("http://127.0.0.1:8000/api/v1/user_list?name="+this.q);
    }
  },
    
  
     
    }

   
    </script>
    
    <style>
    
    </style>