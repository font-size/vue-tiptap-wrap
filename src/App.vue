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
      msg: `<h1>hello world</h1><ol><li><p>it is a rich editor！</p></li><li><p><strong>how are you？</strong> </p></li></ol><blockquote><p>vue+tiptap+elementUI</p><p><a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/font-size/vue-tiptap-wrap">https://github.com/font-size/vue-tiptap-wrap</a></p></blockquote><p><br></p><img src="https://source.unsplash.com/8xznAGy4HcY/800x400" contenteditable="false" draggable="true"><p><br></p>`,
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