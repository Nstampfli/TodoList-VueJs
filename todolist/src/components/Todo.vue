<template>
  <section class="todo">
    <header class="header">
      <h1 class="todo__mainTitle">TODO</h1>
      <input type="text" class="todo__inputNew" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter=addTodo>
    </header>
    <div class="todo__main">
      <input type="checkbox" class="todo__checkAll" v-model="allDone">
      <ul class="todo__list">
        <li class="todo__elements" v-for="todo in filteredTodos" :key="todo.name" :class="{todo__completed: todo.completed}">
          <div class="todo__element">
            <input class="todo__checkbox" type="checkbox" v-model="todo.completed">
            <label>{{todo.name}}</label>
            <button class="todo__destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo_count">
        <strong>{{remaining}} </strong>tâches à faire
      </span>
      <ul class="todo__filters">
        <li><a href="#" :class="{todo__selected: filter === 'all'}" @click.prevent="filter='all'">Toutes</a></li>
        <li><a href="#" :class="{todo__selected: filter === 'todo'}" @click.prevent="filter='todo'" >A faire</a></li>
        <li><a href="#" :class="{todo__selected: filter === 'done'}" @click.prevent="filter='done'">Faites</a></li>
      </ul>
      <button class="todo__clearCompleted" v-show="completed" @click.prevent="deleteCompleted"> Supprimer les tâches finies</button>
    </footer>
  </section>
</template>

<script>
export default {
  data(){
    return{
      todos:[{
        name: 'tache de test',
        completed: false
      }],
      newTodo:'',
      filter:'all'
    }
  },
  methods: {
    addTodo(){
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo =''
    },
    deleteTodo(todo){
      this.todos = this.todos.filter( element => element !==todo)
    },
    deleteCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  computed: {
    allDone: {
      get (){
       return this.remaining === 0
      },
      set(value){
          this.todos.forEach(todo => {
            todo.completed = value
          })
      }
    },
    remaining (){
       return this.todos.filter(todo => !todo.completed).length
    },
    completed(){
       return this.todos.filter(todo => todo.completed).length
    },
    hasTodos(){
      return this.todos.length >0
    },
    filteredTodos(){
      if(this.filter ==='todo'){
        return this.todos.filter( todo => !todo.completed)
      } else if ( this.filter ==='done'){
         return this.todos.filter( todo => todo.completed)
      }
      return this.todos
    }
  }
}
</script>

<style lang="scss">
.todo{
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  &__main{
    width: 100%;
  }
  &__list{
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  &__elements{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 80px;
    width: 80%;
    border: 1px solid black
  }
  &__completed{
    color: aqua;
  }
  &__filters{
    display: flex;
  }
  &__selected{
    color: red;
  }
}
</style>