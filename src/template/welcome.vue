<script lang="ts" setup>
import { ref } from 'vue'

const msg = ref('Hello World!')
</script>

<template>
  <h1>{{ msg }}</h1>

  <!--
    1. Error when using import-map with node_modules:
      Missing ref owner context. 'ref' cannot be used on hoisted VNodes. A VNode with 'ref' must be created inside the render function.
      at <ElInput modelValue="Hello World!" onUpdate:modelValue=fn>
      at <Repl>

      Steps to reproduce: src/template/welcome.vue lines 34-35

    2. Rendering failed when using import-map with CDN link.

      Steps to reproduce: src/template/welcome.vue lines 37-38

    3. Potential listener leak detected:
      Uncaught Error: [001] potential listener LEAK detected, having 200 listeners already. MOST frequent listener (1):
      Error
          at Stacktrace.create (MonacoEditor-CSdcTlIW.js?v=05f73935:2666:21)
          at LanguageSelection._event [as _onDidChangeLanguages] (MonacoEditor-CSdcTlIW.js?v=05f73935:2779:46)
          at get onDidChange (MonacoEditor-CSdcTlIW.js?v=05f73935:165517:35)
          ...
  -->
  <el-input v-model="msg" placeholder="el-input" />
</template>
