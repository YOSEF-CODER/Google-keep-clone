<script setup>

import {ref,onMounted,computed,watch} from 'vue'

const todos = ref([])
const name = ref('')

const input_title = ref('')
const input_content = ref('')
const input_category=ref(null)



const todos_asc=computed(() => {
  return todos.value.sort((a,b) => {
    return b.createdAt - a.createdAt
  })
})


const addTodo = () => {

  if (input_title.value.trim() === '' || input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  todos.value.push({

    title: input_title.value,
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()

  })

  input_title.value = ''
  input_content.value = ''
  input_category.value = null
  
}


const removeTodo=(todo) => {
  return todos.value = todos.value.filter((t) => {
    return t!==todo
    
  })
}


watch(todos,newVal => {
  localStorage.setItem('todos',JSON.stringify(newVal))
},{deep:true})


watch(name,(newVal) => {
  localStorage.setItem('name',newVal)
})


onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value=JSON.parse(localStorage.getItem('todos')) || [] 
})
</script>

<template>

    <header>
          <nav>

            <div class="logo">
              <img src="../assets/img/files.png" alt="">

               <h1>Todo Lists </h1>
            </div>
            
             

              <div class="search">
         
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
    <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z" />
  </svg>
  

                <input type="text" placeholder="search">
              </div>


              
      <h2 class="title">
        What's up,<input type="text" placeholder="Name here..." v-model="name">
      </h2>
  
          </nav>
      </header>






 <main class="app">
  


  <section class="create-todo cardd">
    <h3>Create A Todo List</h3>

    <form @submit.prevent="addTodo">

    


     <input type="text" placeholder="Title" v-model="input_title">
  <textarea type="text" rows="5" placeholder="Content..." v-model="input_content"></textarea>  



      <h4>Pick a category</h4>


     <div class="options">

  					<label>
  						<input 
  							type="radio" 
  							name="category" 
  							id="category1" 
  							value="business"
  							v-model="input_category" />
  						<span class="bubble business"></span>
  						<div>Business</div>
  					</label>

  					<label>
  						<input 
  							type="radio" 
  							name="category" 
  							id="category2" 
  							value="personal"
  							v-model="input_category" />
  						<span class="bubble personal"></span>
  						<div>Personal</div>
  					</label>

  				</div>


      <input type="submit" value="Add Todo" >
    </form>
  </section>


 <section class="todo-list">
  <h3>Todo Lists</h3>

  <div class="list">
    <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`" :key="todo.createdAt">


      <label>
        <input type="checkbox" v-model="todo.done">
        <span :class="`bubble ${todo.category}`"></span>
      </label>

      <div class="todo-content">
        <input type="text" v-model="todo.title">
        <input type="text" v-model="todo.content">
      </div>


      <div class="actions">
        <button class="delete" @click="removeTodo(todo)">Delete</button>
      </div>
    </div>
  </div>
 </section>
 </main>
</template>




<style scoped>
header {
  width: 100%;
  height: 4rem;
  
  display: flex;
  justify-content: center;
  align-items: center;

  border-bottom: 1px solid rgb(172, 193, 208);
}

header a {
  text-decoration: none;
  color: #f391e3;
  display: inline-block;
  padding: 0.75rem 1.5rem;
  border: 1px solid transparent;
}

a:active,
a:hover,
a.router-link-active {
  border: 1px solid #f391e3;
}

h1 {
  margin: 0;
}

h1 a {
  color: white;
  margin: 0;
}

h1 a:hover,
h1 a:active,
h1 a.router-link-active {
  border-color: transparent;
}

header nav {
  width: 90%;
  margin: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

li {
  margin: 0 0.5rem;
}


.search{
border: 1px solid rgb(172, 193, 208);
  height: 2.5rem;
  width: 25rem;
  font-size: large;
  padding: 0.5rem 1rem;
  display: flex;
  flex-direction: row;
  gap: 0.5rem;
  border-radius: 5px;

}

.search svg{
  cursor: pointer;
}

.search input{
  width: 100%;
  
}

.logo{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.logo img{
  height: 2rem;
}



h2.title {
    display: flex;
    font-size: 1.5rem;
    
}

h2.title input {
    margin-left: 0.5rem;
    flex: 1 1 0%;
    min-width: 0;
}

h2.title,
h2.title input {
    color: var(--dark);
    font-size: 1.5rem;
    font-weight: bold;
}

h2.title input {
    color: #00C3FF;
}
</style>
