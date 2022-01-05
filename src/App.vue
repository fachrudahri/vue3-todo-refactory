<template>
  <div class="container">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple todo App</h5>
        <div class="row">
          <div class="col-10">
            <input type="text" class="form-control" v-model="todo" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button class="btn btn-success justify-content-end" @click="add">Add</button>
          </div>
        </div>
        <list :todos="todos.lists" @destroy="deleteTodo" @done="doneTodo" />
        <small>total todo: {{ totalTodo }}</small> || <small>done todo: {{ todoDone }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted, computed } from 'vue';
import List from './components/List.vue';

export default {
  components: {List},
  setup() {
    const todo = ref("")
    const todos = reactive({
      lists: []
    });

    onMounted(() => {
      if(localStorage.key('todos')) {
        todos.lists = JSON.parse(localStorage.getItem("todos"))
      } else {
        localStorage.setItem('todos', JSON.stringify(todos.lists))
      }
    })
    
    const add = () => {
      todos.lists.unshift({
        status: false,
        activity: todo.value
      })
      todo.value = ""
      saveToLocalStorage()
    }

    const deleteTodo = (todoIndex) => {
      todos.lists = todos.lists.filter((item, index) => {
        if(index != todoIndex) {
          return item
        }
      })
      saveToLocalStorage()
    }

    const doneTodo = (todoIndex) => {
      todos.lists = todos.lists.filter((item, index) => {
        if(index == todoIndex) {
          item.status = !item.status
        }
        return item
      })
      saveToLocalStorage()
    }

    const totalTodo = computed(() => {
      return todos.lists.length
    })

    const todoDone = computed(() => {
      return todos.lists.filter(item => item.status).length
    })

    const saveToLocalStorage = () => {
      localStorage.setItem('todos', JSON.stringify(todos.lists))
    }

    return { todo, todos, add, deleteTodo, doneTodo, totalTodo, todoDone }
  }
}
</script>
