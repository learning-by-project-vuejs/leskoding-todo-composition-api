<template>
  <!-- Navbar -->
  <navbar />
  <div class="container">
    <div class="row mt-4">
      <div class="col-md-8 offset-2">
        <div class="input-group mb-3">
          <input type="text" class="form-control" v-model="todo" placeholder="Add to do" @keyup.enter="add">
          <button class="btn btn-outline-primary" type="button" @click="add">Add</button>
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-md-8 offset-2">
        <list 
          :todos="list"
          @emitDone="done"
          @emitDestroy="destroy"
        />
        <small>Total To Do : {{ totalTodo }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted, computed } from 'vue'
import Navbar from './components/Navbar.vue';
import List from './components/List.vue';

export default {
  components: {Navbar, List},
  setup() {
    let todo = ref("")
    let todos = reactive({
      list: []
    })

    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : []
    })

    const totalTodo = computed(() => {
      return todos.length
    })

    const add = () => {
      todos.list.unshift({ activity: todo.value, isDone: false })
      todo.value = ""
      saveToLocalStorage()
    }

    const done = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone
        }

        return item
      })
      saveToLocalStorage()
    }

    const destroy = (todoIndex) => {
       todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      })
      saveToLocalStorage()
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('todos', JSON.stringify(todos.list))
    }
    
    return { todo, ...toRefs(todos), totalTodo, add, done, destroy }
  }

}
</script>