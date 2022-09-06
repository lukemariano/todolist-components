<template>
    <div class="container">
      <div class="conteudo" v-show="!editableMode">
        <div v-show="!createdMode" class="box orange" v-for="task in tasks" :key="task.id">
          <h2 class="task-title">{{ task.title }} | {{ task.date }}</h2>
          <p>{{ task.description }}</p>
          <button class="button buttonColor" @click="deleteTask(task.id)">Delete</button>
          <button class="button buttonColor" @click="callEdit(task.id)">Edit</button>
        </div>
        <div v-show="createdMode" class="newTask">
            <h2 class="task-title">New task</h2>
            <form id="task-form" @submit="createTask">
                <div class="input-container">
                    <label for="taskTitle">Title:</label>
                    <input type="text" v-model="taskTitle" name="taskTitle" id="taskTitle" placeholder="Digite o título da tarefa">
                </div>
                <div class="input-container">
                    <label for="taskDescription">Description:</label>
                    <input type="text" v-model="taskDescription" name="taskDescription" id="taskDescription" placeholder="Descrição da tarefa">
                </div>
                <div class="input-container">
                    <label for="taskEmail">Email:</label>
                    <input type="email" v-model="taskEmail" name="taskEmail" id="taskEmail" placeholder="Digite o seu email">
                </div>
                <div class="input-container">
                    <label for="taskDate">Due to:</label>
                    <input type="date" v-model="taskDate" name="taskDate" id="taskDate">
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Submit Task">
                </div>
            </form>
            <div class="input-container">
                <button class="button" @click="createdMode = false">Back</button>
            </div>
       </div>
      </div>
      <div class="conteudo" v-show="editableMode">
        <h2 class="task-title">Edit task</h2>
            <form id="task-form" @submit="sendEdit">
                <div class="input-container">
                    <label for="taskTitle">Title:</label>
                    <input type="text" v-model="taskTitle" name="taskTitle" id="taskTitle" placeholder="Digite o título da tarefa">
                </div>
                <div class="input-container">
                    <label for="taskDescription">Description:</label>
                    <input type="text" v-model="taskDescription" name="taskDescription" id="taskDescription" placeholder="Descrição da tarefa">
                </div>
                <div class="input-container">
                    <label for="taskEmail">Email:</label>
                    <input type="email" v-model="taskEmail" name="taskEmail" id="taskEmail" placeholder="Digite o seu email">
                </div>
                <div class="input-container">
                    <label for="taskDate">Due to:</label>
                    <input type="date" v-model="taskDate" name="taskDate" id="taskDate">
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Edit Task">
                </div>
            </form>
            <div class="input-container">
                <button class="button" @click="exitEditMode">Back</button>
            </div>
      </div>
    </div>
    <div v-show="!editableMode">
        <button v-show="!createdMode" class="button-create" @click="createdMode = true">+ Create Task +</button>
    </div>
  </template>
  
  <script>
    export default{
        name:'Dashboard',
        data(){
            return{
                tasks:{},
                idEdit: null,
                editTask:{},
                editableMode: false,
                createdMode: false,
                taskTitle: null,
                taskDescription: null,
                taskEmail: null,
                taskDate: null
            }
        },
        methods: {
            async getTasks(){
                const req = await fetch("http://localhost:3000/tasks")
                const response =  await req.json()
                this.tasks = response
            },
            async createTask() {
                
                const data = {
                    title: this.taskTitle,
                    description: this.taskDescription,
                    email: this.taskEmail,
                    date: this.taskDate,
                };
                const dataJson = JSON.stringify(data);
                const req = await fetch("http://localhost:3000/tasks", {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: dataJson,
                });
                const response = await req.json();

                 // limpar os campos após envio
                 this.taskTitle = null;
                 this.taskDescription = null;
                 this.taskEmail = null;
                 this.taskDate = null;

                 this.createdMode = false;
            },
            async deleteTask(id){
                const req = await fetch(`http://localhost:3000/tasks/${id}`, {
                    method: "DELETE",
                });

                this.getTasks()
            },
            async callEdit(id){

                const req = await fetch(`http://localhost:3000/tasks/${id}`);

                const response = await req.json();

                this.editTask = response

                this.taskTitle = this.editTask.title;
                this.taskDescription = this.editTask.description;
                this.taskEmail = this.editTask.email;
                this.taskDate = this.editTask.date;

                this.idEdit = id;

                this.editableMode = true;
            },
            async sendEdit(){
                
            const data = {

                title: this.taskTitle,
                description: this.taskDescription,
                email: this.taskEmail,
                date: this.taskDate,
            };
            
            const dataJson = JSON.stringify(data);

            const req = await fetch(`http://localhost:3000/tasks/${this.idEdit}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson,
            })

            // limpar os campos após envio
            this.taskTitle = null;
            this.taskDescription = null;
            this.taskEmail = null;
            this.taskDate = null;


            this.editableMode = false;


            this.getTasks();
                

            },
            async exitEditMode(){
                // limpar os campos após envio
                this.taskTitle = null;
                this.taskDescription = null;
                this.taskEmail = null;
                this.taskDate = null;

                this.editableMode = false;
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


.button-create {
background-color:hsl(34, 97%, 64%); /* Green */
border: none;
color: black;
padding: 16px 32px;
text-align: center;
text-decoration: none;
display: inline-block;
font-size: 16px;
margin: 4px 2px;
margin-top: 15px;
margin-bottom: 15px;
transition-duration: 0.4s;
cursor: pointer;
position: fixed;
bottom: 30px;
right: 0;
}

.button-create:hover{
    background-color: #555555;
    color: white; 
}

#task-form{
    max-width: 400px;
    margin: 0 auto;
}
.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label{
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;
    text-align: left;
}

input{
    height: 40px;
    padding: 10px;
}

.submit-btn{
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    /* margin: 0 auto; */
    cursor: pointer;
    transition: .5s;
}
.submit-btn:hover{
    background-color: transparent;
    color:#222;
}

.input-container .button {
    margin: 0 auto;
    width: 20%;
    color: #FCBA03;
    font-size: 14px;
    background-color: transparent;
    border: 1px solid #FCBA03;
    transition: .5s;
}

.input-container .button:hover{
    background-color: #222;
}
</style>
  