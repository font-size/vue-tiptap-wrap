<template>
  <div id="app">
    <tiptap v-model="msg" :setImage="setImage" :action="action" :headers="headers" :params="params" :accept="accept"/>
  </div>
</template>

<script>
import tiptap from './components/index'

export default {
  name: 'App',
  components: {
    tiptap
  },
  data() {
    return {
      msg: '',
      action: '',
      headers: {
        Authorization: '',
      },
      params: {
        storeId: '123',
      },
      accept: 'image/*',
    }
  },
  methods:{
    setImage(editor, data) {
      editor.chain().focus().setImage({ src: data.data.file_url }).run();
    }
  }
}
</script>