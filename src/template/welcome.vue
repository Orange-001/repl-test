<script setup>
import 'element-plus/dist/index.css'
import 'element-plus/theme-chalk/dark/css-vars.css'
import { getCurrentInstance } from 'vue'
import { ref } from 'vue'
import ElementPlus from 'element-plus'

const instance = getCurrentInstance()
instance.appContext.app.use(ElementPlus)

const msg = ref('Hello World!')
</script>

<template>
  <h1>{{ msg }}</h1>
  <el-input v-model="msg" />
</template>
