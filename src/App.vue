<template>
  <div>
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader @addTodo="addTodo"/>
        <TodoMain :todos="todos"/>
        <TodoFooter :todos="todos"
                    :deleteCompleteItems="deleteCompleteItems"
                    :selectAllTodos="selectAllTodos"/>
      </div>
    </div>

  </div>
</template>

<script>
  import Pubsub from 'pubsub-js'
import Header from './components/Header'
import Main from './components/Main'
import Footer from './components/Footer'
import storageUtils from './util/storageUtils'
export default{
  data () {
    return {
      todos: storageUtils.readTodos()
    }
  },

  mounted () {
    Pubsub.subscribe('deleteTodo',(msg,index)=>{
      this.deleteTodo(index)
    })
  },

  methods: {
    addTodo (todo) {
      this.todos.unshift(todo)
    },
    deleteTodo (index) {
      this.todos.splice(index,1)
    },
    deleteCompleteItems () {
      this.todos=this.todos.filter(todo => !todo.complete)
    },
    selectAllTodos (isCheck) {
      this.todos.forEach(todo => todo.complete=isCheck)
    }
  },
  watch: {
    todos: {
      deep: true,
      handler: function (value) {
        // localStorage.setItem('todos_key',JSON.stringify(value))
        storageUtils.saveTodos(value)
      }
    }
  },
  components: {
    TodoHeader: Header,
    TodoMain: Main,
    TodoFooter: Footer
  }
}
</script>

<style scoped>
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
</style>
