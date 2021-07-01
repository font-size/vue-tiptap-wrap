# vue-tiptap-wrap

[![npm](https://img.shields.io/npm/dt/vue-tiptap-wrap)](https://www.npmjs.com/package/vue-tiptap-wrap)
[![npm](https://img.shields.io/npm/dw/vue-tiptap-wrap)](https://www.npmjs.com/package/vue-tiptap-wrap)
[![npm](https://img.shields.io/npm/v/vue-tiptap-wrap)](https://www.npmjs.com/package/vue-tiptap-wrap)

![editor](https://raw.githubusercontent.com/font-size/vue-tiptap-wrap/main/img/editor.png)

## start

```
npm i vue-tiptap-wrap
```

and in your page

```
import tiptap from 'vue-tiptap-wrap'

<tiptap v-model="msg">
```

## upload

if you want to upload images

```

<tiptap v-model="msg" :setImage="setImage" :action="action" :headers="headers" :params="params" :accept="accept"/>

```

example

```
data() {
    return {
        action: 'https://www.example.com/upload',
        headers: {
            Authorization: 'abcdefg',
        },
        params: {
            storeId: '123456',
        },
        accept: 'image/*',
    }
},
methods:{
    setImage(editor, data) {
      editor.chain().focus().setImage({ src: data.file_url }).run();
    }
}

```

see upload params detail in ElementUI
<a href="https://element.eleme.cn/#/en-US/component/upload" target="_blank">https://element.eleme.cn/#/en-US/component/upload</a>

## resources

<a href="https://www.tiptap.dev/" target="_blank">https://www.tiptap.dev/</a><br>
<a href="https://element.eleme.cn/#/en-US/component/upload" target="_blank">https://element.eleme.cn/</a><br>

## License

MIT
