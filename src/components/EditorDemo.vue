<template>
  <div id="editor">
    <div :id="richTextId"></div>
  </div>
</template>

<script>
import E from 'wangeditor'
export default {
  props: {
    richText: {
      type: String,
      required: false
    },
    richTextId: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      editor: null
    }
  },
  mounted() {
    this.editor = new E(`#${this.richTextId}`)
    this.editor.config.menus = [
      'italic',
      'underline',
      'strikeThrough',
      'indent',
      'lineHeight',
      'foreColor',
      'list',
      'emotions',
      'todo',
      'quote',
      'table',
      'splitLine',
      'undo',
      'redo',
      'code'
    ]
    this.editor.config.onchange = html => {
      this.$emit('richTextValue', html)
    }
    // this.$nextTick(() => {
    // console.log(this.richText,'richText');

    // })
    console.log(this.richText, 'this.richText')
    this.editor.create()
    this.editor.txt.html(this.richText)
  },
  beforeUnmount() {
    this.editor.destroy()
  }
}

</script>
