<template>
  <div id="app">
    {{msg}}
    <tiptap v-model="msg" :height="750" />
  </div>
</template>

<script>
import tiptap from './components/tiptap/index'

export default {
  name: 'App',
  components: {
    tiptap
  },
  data() {
    return {
      msg: '123'
    }
  }
}
</script>

<style>
</style>
