<template>
  <div class="hello">
    <!-- <div id="editor"></div> -->
    <!-- <div style="border: 1px solid #ccc">
      <Toolbar style="border-bottom: 1px solid #ccc" :editor="editorRef" :defaultConfig="toolbarConfig" :mode="mode" />
      <Editor style="height: 500px; overflow-y: hidden;" v-model="valueHtml" :defaultConfig="editorConfig" :mode="mode"
        @onCreated="handleCreated" />
    </div> -->

    <div id="editor—wrapper">
      <div id="toolbar-container"><!-- 工具栏 --></div>
      <div id="editor-container"><!-- 编辑器 --></div>
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
import { onMounted } from 'vue'


onMounted(() => {

  const editorConfig = {
    placeholder: 'Type here...',
    onChange(editor) {
      const html = editor.getHtml()
      console.log('editor content', html)
      // 也可以同步到 <textarea>
    }
  }

  const editor = createEditor({
    selector: '#editor-container',
    html: '<p><br></p>',
    config: editorConfig,
    mode: 'default', // or 'simple'
  })

  const toolbarConfig = {}

  // eslint-disable-next-line no-unused-vars
  const toolbar = createToolbar({
    editor,
    selector: '#toolbar-container',
    config: toolbarConfig,
    mode: 'default', // or 'simple'
  })

})

</script>
