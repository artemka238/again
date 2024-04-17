<template lang="">
    <div>
        <main>

          <nav class="navbar navbar-expand navbar-dark bg-dark" aria-label="Second navbar example">
    <!-- <div class="container-fluid">
      <a class="navbar-brand" href="#">Always expand</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarsExample02" aria-controls="navbarsExample02" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarsExample02">
        <ul class="navbar-nav me-auto">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item">
            <p class="nav-link" href="#">{{ username }}</p>
          </li>
          <li class="nav-item">
            <p class="nav-link" href="#">{{ balance }}</p>
          </li>
        </ul>
        <form role="search">
          <input class="form-control" type="search" placeholder="Search" aria-label="Search">
        </form>
      </div>
    </div> -->
    <li class="nav-item">
                    <p class="nav-link" href="#"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-person-circle" viewBox="0 0 16 16">
  <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0"/>
  <path fill-rule="evenodd" d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1"/>
</svg> {{ username }}</p>
                </li>
                <li class="nav-item">
                  
                    <p class="nav-link" href="#"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-bank" viewBox="0 0 16 16">
  <path d="m8 0 6.61 3h.89a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.5.5H15v7a.5.5 0 0 1 .485.38l.5 2a.498.498 0 0 1-.485.62H.5a.498.498 0 0 1-.485-.62l.5-2A.5.5 0 0 1 1 13V6H.5a.5.5 0 0 1-.5-.5v-2A.5.5 0 0 1 .5 3h.89zM3.777 3h8.447L8 1zM2 6v7h1V6zm2 0v7h2.5V6zm3.5 0v7h1V6zm2 0v7H12V6zM13 6v7h1V6zm2-1V4H1v1zm-.39 9H1.39l-.25 1h13.72z"/>
</svg> {{ balance }}</p>
                </li>
  </nav>

<div class="album py-5 bg-body-tertiary">
  <div class="container">

    <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
      <div class="col" v-for = "cat in cats" :key = "cat.id">
        <div class="card shadow-sm">
            <img v-if = 'cat.image' :src="`http://127.0.0.1:8000${cat.image}`">
            <img v-else src="https://img.tourister.ru/files/2/4/3/4/1/8/4/4/original.jpg">
          <div class="card-body">
            <p>{{ cat.name }}</p>
            <p>{{ cat.user.username }}</p>
            <p>{{ cat.breed }}</p>
            <p>{{ cat.age }}</p>
            <p>{{ cat.cost }}</p>
            <p class="card-text">This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.</p>
            <div class="d-flex justify-content-between align-items-center">
              <div class="btn-group">
                <button @click = "buycat(cat.id)">
                  Buy
                </button>
              </div>
              <small class="text-body-secondary">9 mins</small>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

</main>
        <!-- <div v-for = "cat in cats">
            <p>{{ cat.name }}</p>
            <p>{{ cat.breed }}</p>
            <p>{{ cat.age }}</p>
            <p>{{ cat.cost }}</p>
            <img v-if = 'cat.image' :src = 'cat.image'>
        </div> -->
                {{ err }}
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return{
            cats:"",
            err:"",
            balance:"",
            username:""
        }
    },
    methods:{
        LoadRooms(){
            console.log(1)
            // axios - функция для запросов к сайтам
            axios({
                url:"http://127.0.0.1:8000/morecats/cats_shop/",
                method:"get",
                // responseType:'json',
            }).then(response => {
              console.log(response)
                console.log(response.status)
                this.cats = response.data.data
            }).catch(error => {
                console.log(error.response)
                if (error.response.status === 403)
                {
                    this.err = "you have not permission, buy 10 cats or leave"
                }
            })
            },
            CreateRoom(){
            // axios - функция для запросов к сайтам
            axios({
                url:"http://127.0.0.1:8000/morecats/cats/",
                method:"post",
                // responseType:'json',
            }).then(response => {
                console.log(response)
                // this.rooms = response.data.data.data
                this.LoadRooms()
            }).catch(error => {
                console.log(error)

            })
            },
            buycat(cat){
              console.log(cat)
              const dataform = new FormData
              dataform.append('id_cat',cat)
              axios({
                url:"http://127.0.0.1:8000/morecats/cats/buy/",
                method:"post",
                data:dataform
                
                
            
                // responseType:'json',
            }).then(response => {
              console.log(response)
                console.log(response.status)
            }).catch(error => {
                console.log(error.response)
                if (error.response.status === 403)
                {
                    this.err = "you have not permission, buy 10 cats or leave"
                }
            })
            },
            getinfo(){
              axios({
                url:"http://127.0.0.1:8000/morecats/info",
                method:"get",
                // responseType:'json',
            }).then(response => {
              console.log(response)
                console.log(response.status)
                this.username = response.data.user.username
                this.balance = response.data.balance
            }).catch(error => {
                console.log(error.response)
                if (error.response.status === 403)
                {
                    this.err = "you have not permission, buy 10 cats or leave"
                }
            })
            }
        },
        // ФУНКЦИЯ КОТОРАЯ ВЫПОЛНЯЕТСЯ ПРИ ЗАГРУЗКЕ СТРАНИЦЫ
        created(){
            // в каждый запрос добавляем токен
            axios.defaults.headers['authorization'] = 'Token ' + sessionStorage.getItem('authtoken')
            this.LoadRooms()
            this.getinfo()
        }

}
</script>

<style lang="">
    
</style>