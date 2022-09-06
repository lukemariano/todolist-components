<template>
    <div class="container">
      <div class="conteudo">
        <div class="box orange" v-for="task in tasks" :key="task.id">
          <h2 class="task-title">{{ task.title }} | {{ task.dueTo }}</h2>
          <p>{{ task.description }}</p>
          <button class="button buttonColor">Delete</button>
          <button class="button buttonColor">Edit</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
    export default{
        name:'Dashboard',
        data(){
            return{
                tasks:{}
            }
        },
        methods: {
            async getTasks(){
                const req = await fetch("http://localhost:3000/tasks")
                const response =  await req.json()
                this.tasks = response
            },
        },
        mounted(){
            this.getTasks()
        },
    }
  
  </script>
  
  <style scoped>
  
    #app .container{
      height: 82.3%;
      display: flex;
      justify-content: center;
      margin-top: 50px;
    }
    .conteudo{
      text-align: center;
      width: 70%;
    }
  
    .box {
      border-radius: 5px;
      width: 100%;
      box-shadow: 0px 30px 40px -20px hsl(229, 6%, 66%);
      padding: 30px;
      margin: 20px;  
      text-align: left;
      color: rgba(0, 0, 0, 0.849);
      margin-bottom: 50px;
  }
  
  .orange {
      border-top: 3px solid  hsl(34, 97%, 64%);
  }
  
  .task-title{
    margin-bottom: 45px;
    font-size: 22px;
    color: hsl(0, 0%, 0%);
    letter-spacing: 2px;
  }
  
  .button {
    border: none;
    color: white;
    padding: 10px 28px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 13px;
    margin: 4px 2px;
    margin-top: 45px;
    margin-right: 10px;
    transition-duration: 0.4s;
    cursor: pointer;
  }
  
  .buttonColor {
    background-color: white;
    color: black;
    border: 2px solid #555555;
  }
  
  .buttonColor:hover{
    background-color: #555555;
    color: white;
  }
  </style>
  