<template>
  <div>
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader :addTodo="addTodo"/>
        <TodoMain :todos="todos" :deleteTodo="deleteTodo"/>
        <TodoFooter :todos="todos"
                    :deleteCompleteItems="deleteCompleteItems"
                    :selectAllTodos="selectAllTodos"/>
      </div>
    </div>

  </div>
</template>

<script>
import Header from './components/Header'
import Main from './components/Main'
import Footer from './components/Footer'

export default{
  data () {
    return {
      todos: [
        {title: '吃饭', complete: false},
        {title: '睡觉', complete: false},
        {title: '打豆豆', complete: false}
      ]
    }
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
