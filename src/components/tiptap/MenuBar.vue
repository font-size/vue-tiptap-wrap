<template>
  <div>
    <template v-for="(item, index) in items">
      <div class="divider" v-if="item.type === 'divider'" :key="index" />
      <uploadfile
        :key="index"
        v-else-if="item.type === 'img-upload'"
        :action="action"
        :Authorization="Authorization"
        :storeId="storeId"
        :type="type"
        :isEditorBar="true"
        :autoUpload="true"
        :showFileList="false"
        @upload="uploadSuccess"
      >
        <menu-item v-bind="item" />
      </uploadfile>
      <menu-item v-else :key="index" v-bind="item" />
    </template>
  </div>
</template>

<script>
import uploadfile from './UploadFiles';
import MenuItem from './MenuItem.vue';

export default {
  components: {
    MenuItem,
    uploadfile,
  },

  props: {
    editor: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      action: process.env.VUE_APP_BASE_API + 'pms/file/upload',
      Authorization: localStorage.getItem('loginToken'),
      storeId: localStorage.getItem('store_id'),
      type: ['image/png', 'image/jpeg', 'image/jpg'],
      items: [
        {
          icon: 'arrow-go-back-line',
          title: '撤销',
          action: () => this.editor.chain().focus().undo().run(),
        },
        {
          icon: 'arrow-go-forward-line',
          title: '取消撤销',
          action: () => this.editor.chain().focus().redo().run(),
        },
        {
          type: 'divider',
        },
        {
          icon: 'bold',
          title: '加粗',
          action: () => this.editor.chain().focus().toggleBold().run(),
          isActive: () => this.editor.isActive('bold'),
        },
        {
          icon: 'italic',
          title: '斜体',
          action: () => this.editor.chain().focus().toggleItalic().run(),
          isActive: () => this.editor.isActive('italic'),
        },
        {
          icon: 'strikethrough',
          title: '删除线',
          action: () => this.editor.chain().focus().toggleStrike().run(),
          isActive: () => this.editor.isActive('strike'),
        },
        {
          icon: 'link',
          title: '超链接',
          action: this.setLink,
          isActive: () => this.editor.isActive('link'),
        },
        {
          type: 'divider',
        },
        {
          icon: 'gallery-upload-line',
          title: '上传图片',
          type: 'img-upload',
        },
        {
          type: 'divider',
        },
        {
          icon: 'h-1',
          title: 'Heading 1',
          action: () => this.editor.chain().focus().toggleHeading({ level: 1 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 1 }),
        },
        {
          icon: 'h-2',
          title: 'Heading 2',
          action: () => this.editor.chain().focus().toggleHeading({ level: 2 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 2 }),
        },
        {
          icon: 'h-3',
          title: 'Heading 3',
          action: () => this.editor.chain().focus().toggleHeading({ level: 3 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 3 }),
        },
        {
          icon: 'h-4',
          title: 'Heading 4',
          action: () => this.editor.chain().focus().toggleHeading({ level: 4 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 4 }),
        },
        {
          icon: 'h-5',
          title: 'Heading 5',
          action: () => this.editor.chain().focus().toggleHeading({ level: 5 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 5 }),
        },
        {
          icon: 'h-6',
          title: 'Heading 6',
          action: () => this.editor.chain().focus().toggleHeading({ level: 6 }).run(),
          isActive: () => this.editor.isActive('heading', { level: 6 }),
        },
        // {
        //   icon: 'paragraph',
        //   title: 'Paragraph',
        //   action: () => this.editor.chain().focus().setParagraph().run(),
        //   isActive: () => this.editor.isActive('paragraph'),
        // },
        {
          icon: 'list-unordered',
          title: '无序列表',
          action: () => this.editor.chain().focus().toggleBulletList().run(),
          isActive: () => this.editor.isActive('bulletList'),
        },
        {
          icon: 'list-ordered',
          title: '有序列表',
          action: () => this.editor.chain().focus().toggleOrderedList().run(),
          isActive: () => this.editor.isActive('orderedList'),
        },
        // {
        //   icon: 'code-box-line',
        //   title: 'Code Block',
        //   action: () => this.editor.chain().focus().toggleCodeBlock().run(),
        //   isActive: () => this.editor.isActive('codeBlock'),
        // },
        {
          type: 'divider',
        },
        {
          icon: 'double-quotes-l',
          title: '引用',
          action: () => this.editor.chain().focus().toggleBlockquote().run(),
          isActive: () => this.editor.isActive('blockquote'),
        },
        {
          type: 'divider',
        },
        {
          icon: 'text-wrap',
          title: '换行',
          action: () => this.editor.chain().focus().setHardBreak().run(),
        },
        {
          icon: 'format-clear',
          title: '清除格式',
          action: () => this.editor.chain().focus().clearNodes().unsetAllMarks().run(),
        },
      ],
    };
  },
  methods: {
    uploadSuccess(data) {
      this.editor.chain().focus().setImage({ src: data.file_url }).run();
      // this.editor.commands.insertContent(`<a href='${data.file_url}'>1234567654321</a>`);
    },
    setLink() {
      if (this.editor.isActive('link')) {
        this.editor.chain().focus().unsetLink().run();
      } else {
        const url = window.prompt('请输入URL');
        if (url && url.trim()) {
          this.editor.chain().focus().setLink({ href: url }).run();
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.divider {
  width: 2px;
  height: 1.25rem;
  background-color: rgba(#000, 0.1);
  margin-left: 0.5rem;
  margin-right: 0.75rem;
}
</style>
