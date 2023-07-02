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

const clearAll = () => {
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
  111
  <div class="py-5" style="background-image: url(./q.jpg);">
    <div class="bg-gradient-to-r from-purple-500 to-pink-500 h-screen px-20 mx-20 saturate-50 backdrop-blur-sm">
    <div class="common-layout flex flex-col container px-80 gap-5 font-bold font-black">
    <div class="py-5">Welcome to your to-do-list!
      <el-input v-model="name" placeholder="name here!" class="w-50 py-4" size="large" />
    </div>

      <el-header>
        <div>Create-todo</div>
        <el-input v-model="input_content" placeholder="e.g. make a video" class="w-50 m-2 py-4" size="large" />
      </el-header>

      <el-main>
        <div class="py-2">Pick a category</div>
        <el-radio-group v-model="input_category" size="large" class="flex gap-4 py-4">
          <el-radio-button label="School" value="School" class=""/>
          <el-radio-button label="Home" value="Home" />
          <el-radio-button label="Club" value="Club" />
          <el-radio-button label="Friends" value="Friends" />
        </el-radio-group>
        <div>
        </div>
      </el-main>
      <el-button type="primary" @click="addTodo">Add to do</el-button>
      <el-footer class="space-y-3">
        <div>To do list</div>
        <div class="flex flex-col space-y-5">
          <!-- 创建to-do-list -->
          <div v-for="todo in todos_asc">
            <div class="flex justify-around">
              <el-checkbox-button type="checkbox" v-model="todo.done" size="large">
                <span v-if="todo.done" style="text-decoration: line-through" class="flex-1">{{ todo.content }}</span>
                <span v-else class="flex-1">{{ todo.content }}</span>
              </el-checkbox-button>
              <el-button type="danger" :icon="Delete" circle @click="removeTodo(todo)" class=""></el-button>
            </div>
          </div>
        </div>
        <div class="flex justify-center">
          <el-button type="danger" :icon="Delete" @click="clearAll" class="center w-36 hover:w-40">clear all</el-button>
        </div>
      </el-footer>
    </div>
  </div>
  </div>
  
</template>


<style scoped></style>
