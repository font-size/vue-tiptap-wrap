<template>
  <div>
    <template v-for="(item, index) in items">
      <div class="divider" v-if="item.type === 'divider'" :key="index" />
      <uploadfile
        :key="index"
        v-else-if="item.type === 'upload' && action"
        :action="action"
        :headers="headers"
        :params="params"
        :accept="accept"
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

    action: {
      type: String,
      default: '',
    },
    headers: {
      type: Object,
      default: () => {},
    },
    params: {
      type: Object,
      default: () => {},
    },
    accept: {
      type: String,
      default: '',
    },
    setImage: {
      type: Function,
      default: () => {},
    }
  },

  data() {
    return {
      items: [
        {
          icon: 'arrow-go-back-line',
          title: 'Undo',
          action: () => this.editor.chain().focus().undo().run(),
        },
        {
          icon: 'arrow-go-forward-line',
          title: 'Redo',
          action: () => this.editor.chain().focus().redo().run(),
        },
        {
          type: 'divider',
        },
        {
          icon: 'bold',
          title: 'Bold',
          action: () => this.editor.chain().focus().toggleBold().run(),
          isActive: () => this.editor.isActive('bold'),
        },
        {
          icon: 'italic',
          title: 'Italic',
          action: () => this.editor.chain().focus().toggleItalic().run(),
          isActive: () => this.editor.isActive('italic'),
        },
        {
          icon: 'strikethrough',
          title: 'Strike',
          action: () => this.editor.chain().focus().toggleStrike().run(),
          isActive: () => this.editor.isActive('strike'),
        },
        {
          icon: 'link',
          title: 'Link',
          action: this.setLink,
          isActive: () => this.editor.isActive('link'),
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
        {
          icon: 'paragraph',
          title: 'Paragraph',
          action: () => this.editor.chain().focus().setParagraph().run(),
          isActive: () => this.editor.isActive('paragraph'),
        },
        {
          icon: 'list-unordered',
          title: 'BulletList',
          action: () => this.editor.chain().focus().toggleBulletList().run(),
          isActive: () => this.editor.isActive('bulletList'),
        },
        {
          icon: 'list-ordered',
          title: 'OrderedList',
          action: () => this.editor.chain().focus().toggleOrderedList().run(),
          isActive: () => this.editor.isActive('orderedList'),
        },
        {
          icon: 'code-box-line',
          title: 'Code Block',
          action: () => this.editor.chain().focus().toggleCodeBlock().run(),
          isActive: () => this.editor.isActive('codeBlock'),
        },
        {
          type: 'divider',
        },
        {
          icon: 'double-quotes-l',
          title: 'Blockquote',
          action: () => this.editor.chain().focus().toggleBlockquote().run(),
          isActive: () => this.editor.isActive('blockquote'),
        },
        {
          type: 'divider',
        },
        {
          icon: 'text-wrap',
          title: 'Wrap',
          action: () => this.editor.chain().focus().setHardBreak().run(),
        },
        {
          icon: 'format-clear',
          title: 'Format',
          action: () => this.editor.chain().focus().clearNodes().unsetAllMarks().run(),
        },
        {
          icon: 'gallery-upload-line',
          title: 'Upload images',
          type: 'upload',
          customIcon: 'el-icon-picture'
        },
      ],
    };
  },
  methods: {
    uploadSuccess(data) {
      this.setImage(this.editor, data)
      // this.editor.chain().focus().setImage({ src: data.file_url }).run();
    },
    setLink() {
      if (this.editor.isActive('link')) {
        this.editor.chain().focus().unsetLink().run();
      } else {
        const url = window.prompt('URL');
        if (url && url.trim()) {
          this.editor.chain().focus().setLink({ href: url }).run();
        }
      }
    },
  },
};
</script>

<style>
.divider {
  width: 2px;
  height: 1.25rem;
  background-color: rgba(#000, 0.1);
  margin-left: 0.5rem;
  margin-right: 0.75rem;
}
</style>
