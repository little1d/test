<script setup>
import { ref, computed, watch, onMounted } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

const radio = ref('School')

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))



const addTodo = () => {
  if (input_content.value.trim() === '') {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

const clearAll = () =>{
  todos.value = []
}

onMounted(() => {
  name.value = localStorage.getItem('name'),
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

import {
  Check,
  Delete,
  Edit,
  Message,
  Search,
  Star,
} from '@element-plus/icons-vue'
</script>

<template>
  <div class="common-layout flex flex-col container mx-auto relative w-90 h-70">
    <div class="place-content-center">Welcome to your to-do-list!</div>
    <el-input v-model="name" placeholder="name here!" class="w-50 m-2" size="large" />
    <el-container>
      <el-header>
        <div>Create-todo</div>
        <el-input v-model="input_content" placeholder="e.g. make a video" class="w-50 m-2" size="large" />

      </el-header>

      <el-main>
        <div>Pick a category</div>
        <el-radio-group v-model="input_category" size="large">
          <el-radio-button label="School" value="School"/>
          <el-radio-button label="Home" value="Home"/>
          <el-radio-button label="Club" value="Club"/>
          <el-radio-button label="Friends" value="Friends"/>
        </el-radio-group>
        <div>
          {{ input_category }}
        </div>
      </el-main>
      <el-button type="primary" @click="addTodo">Add to do</el-button>
      <el-footer class="space-y-3">
        <div >To do list</div>

        <div class="flex flex-col space-y-5" >

          <div v-for="todo in todos_asc" >

            <div class="todo-content" >
              <input type="checkbox" v-model="todo.done">
              <input type="text" v-model="todo.content">
              <el-button type="danger" :icon="Delete" @click="removeTodo(todo)">delete</el-button>
            </div>
            
          </div>
        </div>
        <el-button type="danger" @click="clearAll">clear all</el-button>

      </el-footer>
    </el-container>
  </div>
</template>


<style scoped>
</style>
