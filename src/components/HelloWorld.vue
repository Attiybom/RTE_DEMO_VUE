<template>
  <div class="hello">
    <!-- <div style="border: 1px solid #ccc">
      <Toolbar style="border-bottom: 1px solid #ccc" :editor="editorRef" :defaultConfig="toolbarConfig" :mode="mode" />
      <Editor style="height: 500px; overflow-y: hidden;" v-model="valueHtml" :defaultConfig="editorConfig" :mode="mode"
        @onCreated="handleCreated" />
    </div> -->

    <div id="editor—wrapper">
      <div class="toolbar">
        <button @click="clearContent">清空</button>
        <button @click="undo">撤销</button>
      </div>
      <div ref="toolbarRef"><!-- 工具栏 --></div>
      <div ref="editorRef" class="editor-wrapper" />
    </div>

  </div>
</template>

<!-- <script setup>
// import '@wangeditor/editor/dist/css/style.css'
// import { onBeforeUnmount, ref, shallowRef, onMounted } from 'vue'
// import { Editor, Toolbar } from '@wangeditor/editor-for-vue'

// // 模式
// const mode = ref('default')

// // 编辑器实例，必须用 shallowRef
// const editorRef = shallowRef()

// // 内容 HTML
// const valueHtml = ref('<p>初始化完成</p>')

// // 模拟 ajax 异步获取内容
// onMounted(() => {
//   setTimeout(() => {
//     valueHtml.value = '<p>请输入内容...</p>'
//   }, 1500)
// })

// const toolbarConfig = {}
// const editorConfig = { placeholder: '请输入内容...' }

// // 组件销毁时，也及时销毁编辑器
// onBeforeUnmount(() => {
//   const editor = editorRef.value
//   if (editor == null) return
//   editor.destroy()
// })

// const handleCreated = (editor) => {
//   editorRef.value = editor // 记录 editor 实例，重要！
// }

</script> -->


<script setup>
import '@wangeditor/editor/dist/css/style.css'
import { createEditor, createToolbar } from '@wangeditor/editor'
import { onMounted, ref, shallowRef, onBeforeUnmount } from 'vue'

// 模式
const mode = ref('default')

// 编辑器实例，必须用 shallowRef
const editorRef = shallowRef()
const toolbarRef = shallowRef()

// dom 方便销毁
let editor = null
let toolbar = null

// 内容 HTML
// createEditor下这种方案并不可行，不能用v-model绑定
// const valueHtml = ref('初始化完成...')


// 外面也可以调api
function clearContent() {
  if (editor == null) return
  editor.clear()
}

function undo() {
  if (editor == null) return
  editor.undo()
}

onMounted(() => {

  const editorConfig = {
    placeholder: 'Type here...',
    onChange(editor) {
      console.log('editor.getText()', editor.getText())
      // 这里就可以调api
    }
  }

  editor = createEditor({
    selector: editorRef.value,
    html: '<p><br></p>',
    config: editorConfig,
    mode: mode.value, // or 'simple'
  })

  const toolbarConfig = {}

  // eslint-disable-next-line no-unused-vars
  toolbar = createToolbar({
    editor,
    selector: toolbarRef.value,
    config: toolbarConfig,
    mode: mode.value, // or 'simple'
  })

  // 模拟 ajax 异步获取内容
  // 假设后端返回html内容
  setTimeout(() => {
    // 做边界处理，防止获取不到dom
    if (editor == null) return
    editor.setHtml('<p>this is ajax content</p>')
  }, 1000)
})



onBeforeUnmount(() => {

  // 销毁编辑器实例
  if (editor == null) return
  editor.destroy()
  editor = null

  // 销毁工具栏实例
  if (toolbar == null) return
  toolbar = null
})

</script>


<style scoped>
.editor-wrapper {
  height: 500px;
  border: none;
  overflow-y: hidden;
}
</style>
