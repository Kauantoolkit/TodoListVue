<script setup>
import { ref, onMounted, computed, watch } from "vue";
const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const addTodo = () => {

  if (input_content.value.trim() === "" || input_category.value === null){
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime() 
  })


}

const todo_asc = computed(()=> todos.value.sort((a,b) => {
  return b.createdAt - a.createdAt
}))


const removeTodo = (todo => {
  todos.value = todos.value.filter(t => t!== todo)
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)})
  

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
  
}, {deep: true})










onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>




<template>
  

<main class="app">
  <section class="saudacao">
    <h2 class="title">
      Olá!, <input type="text" placeholder="nome aqui" v-model="name"/>
    </h2>

  </section>

  <section class="create-todo">

    <h3>Criar uma tarefa</h3>

    <form @submit.prevent="addTodo" >
        <h4>Oq está na sua lista de afazeres?</h4>
        
        <input 
        type="text" 
        placeholder="Tarefa" 
        v-model="input_content">

        <h4>Escolha uma categoria</h4>

        <div class="options">
          <label >
              <input type='radio'
              name="category"
              id="category"
              value="business" v-model = input_category
              />
              <span class="bubble bubblenegocios"></span>
              <div>Negócios</div>
          </label>

          <label >
              <input type='radio'
              name="category"
              id="category1"
              value="pessoal" v-model = input_category
              />
              <span class="bubble bubblepessoal"></span>
              <div>Pessoal</div>

          </label>


        </div>

        <input type="submit" value="Add todo">
        
    </form>

  </section>

  <section class="todo-list">
      <h3>Lista de afazeres</h3>
      <div class="list">
        <div v-for="todo in todo_asc" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
          <label >
            <input type="checkbox" v-model="todo.done"/>
            <span :class="`bubble ${todo.category}`"></span>
            

          </label>
        <div class="todo-content">
          <input type="text" v-model="todo.content">

        </div>
        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Deletar</button>
        </div>

      </div>
      </div>






  </section>


    
</main>
  

</template>


