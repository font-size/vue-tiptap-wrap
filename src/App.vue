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
      action: 'yourAction',
      headers: {
        Authorization: 'yourAuthorization',
      },
      params: {
        storeId: '001',
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