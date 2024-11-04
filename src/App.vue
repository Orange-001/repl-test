<script setup lang="ts">
import { reactive, toRefs, ref, watchEffect } from 'vue'
import {
  mergeImportMap,
  File,
  Repl,
  useStore,
  useVueImportMap,
  type StoreState,
} from '@vue/repl'
import Monaco from '@vue/repl/monaco-editor'
import welcomeCode from './template/welcome.vue?raw'
import tsconfigCode from './template/tsconfig.json?raw'

const replRef = ref<InstanceType<typeof Repl>>()
const previewOptions = {
  customCode: {
    importCode: `
      import 'element-plus/dist/index.css';
      import 'element-plus/theme-chalk/dark/css-vars.css';
      import ElementPlus from 'element-plus';
    `,
    useCode: `
      app.use(ElementPlus);
    `,
  },
}

const { importMap: builtinImportMap, vueVersion } = useVueImportMap({})
const storeState: Partial<StoreState> = toRefs(
  reactive({
    builtinImportMap: mergeImportMap(builtinImportMap.value, {
      imports: {
        'element-plus': '/node_modules/element-plus/dist/index.full.min.mjs',
        'element-plus/': '/node_modules/element-plus/',

        // 'element-plus':
        //   'https://cdn.jsdelivr.net/npm/element-plus/dist/index.full.min.mjs',
        // 'element-plus/': 'https://cdn.jsdelivr.net/npm/element-plus/',
      },
    }),
    vueVersion,
    template: {
      welcomeSFC: welcomeCode,
    },
    files: {
      'tsconfig.json': new File('tsconfig.json', tsconfigCode),
    },
  }),
)
const store = useStore(storeState, location.hash)
watchEffect(() => history.replaceState({}, '', store.serialize()))
</script>

<template>
  <Repl
    ref="replRef"
    :store="store"
    :editor="Monaco"
    theme="dark"
    :previewTheme="true"
    :clearConsole="false"
    :previewOptions="previewOptions"
  />
</template>
