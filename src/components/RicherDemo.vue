<template>
  <div class="richer-demo">
    <div v-for="(item) in Items" :key="item.editor">
      <el-input v-model="item.input" placeholder="请输入内容"></el-input>
      <Editor :richText="item.reservationInstructions" :richTextId="item.editor"
        @richTextValue="val => reviceRichText(val, item.editor)"></Editor>
    </div>
    <button type="primary" @click="add">增加</button>
  </div>
</template>

<script setup>

// eslint-disable-next-line no-unused-vars
import Editor from './EditorDemo.vue';
// eslint-disable-next-line no-unused-vars
import { ref, nextTick, onMounted, computed } from "vue";

const searchForm = ref({
  app: [
    {
      reservationInstructions: '',
      input: ''
    }
  ]
})

const Items = computed(() => {
  return searchForm.value.app.map((item, index) => {
    return {
      reservationInstructions: item.reservationInstructions,
      input: item.input,
      editor: 'editor-' + (index + 1)
    }
  })
})

// const EditorRef = ref(null)
// onMounted(() => {
//   // console.log(EditorRef.value.richText, 'EditorRef');
//   // nextTick(() => {
//   // searchForm.value.reservationInstructions =EditorRef.value.richText

//   // })
// })

const reviceRichText = (val, editorId) => {
  // 从editorId字符串中提取数字部分
  const index = parseInt(editorId.replace('editor-', ''), 10) - 1; // 把'editor-1'转换为0，这样才对应数组的正确索引

  if (searchForm.value.app[index]) {
    searchForm.value.app[index].reservationInstructions = val;
  } else {
    console.error('Invalid editor index or app array:', editorId, searchForm.value.app);
  }
}


const add = () => {
  const newEditorIndex = searchForm.value.app.length + 1;
  searchForm.value.app.push({
    reservationInstructions: '',
    input: '',
    editor: 'editor-' + newEditorIndex
  });
}

</script>
<style lang="scss" scoped></style>
