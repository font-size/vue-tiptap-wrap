<template>
  <div>
    <div
      class="editor"
      v-if="editor"
      :style="{
        height: height + 'px',
      }"
    >
      <menu-bar class="editor__header" :editor="editor" />
      <editor-content class="editor__content" :editor="editor" ref="editor" />
    </div>
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2';
import StarterKit from '@tiptap/starter-kit';
import Image from '@tiptap/extension-image';
import Links from '@tiptap/extension-link';
// 表格相关
import Table from '@tiptap/extension-table';
import TableRow from '@tiptap/extension-table-row';
import TableCell from '@tiptap/extension-table-cell';
import TableHeader from '@tiptap/extension-table-header';

// import Document from '@tiptap/extension-document';
// import Paragraph from '@tiptap/extension-paragraph';
// import Text from '@tiptap/extension-text';
// import Dropcursor from '@tiptap/extension-dropcursor';

import MenuBar from './MenuBar.vue';

// 表格函数
const CustomTableCell = TableCell.extend({
  addAttributes() {
    return {
      // extend the existing attributes …
      ...this.parent?.(),

      // and add a new one …
      backgroundColor: {
        default: null,
        parseHTML: element => {
          return {
            backgroundColor: element.getAttribute('data-background-color'),
          };
        },
        renderHTML: attributes => {
          return {
            'data-background-color': attributes.backgroundColor,
            'style': `background-color: ${attributes.backgroundColor}`,
          };
        },
      },
    };
  },
});

export default {
  components: {
    EditorContent,
    MenuBar,
  },

  data() {
    return {
      editor: null,
      widthType: '1',
    };
  },

  props: {
    value: {
      type: String,
      default: '',
    },
    height: {
      type: Number,
      default: 500,
    },
    width: {
      type: Number,
      default: 0,
    },
  },

  // v-model
  watch: {
    value(value) {
      // HTML
      const isSame = this.editor.getHTML() === value;

      // JSON
      // const isSame = this.editor.getJSON().toString() === value.toString()

      if (isSame) {
        return;
      }

      this.editor.commands.setContent(this.value, false);
    },
  },

  mounted() {
    // editor init
    this.editor = new Editor({
      extensions: [
        StarterKit,
        Image,
        Links,
        CustomTableCell,
        Table.configure({
          resizable: true,
        }),
        TableRow,
        TableHeader,
      ],
      content: this.value,
      onUpdate: () => {
        this.$emit('input', this.editor.getHTML());
      },
    });
  },

  methods: {
  },

  beforeUnmount() {
    this.editor.destroy();
    // this.provider.destroy();
  },
};
</script>

<style lang="scss" scoped>
.editor {
  display: flex;
  flex-direction: column;
  // max-height: 400px;
  color: #0d0d0d;
  background-color: #fff;
  border: 1px solid #dcdfe6;
  border-radius: 0.75rem;

  &__header {
    display: flex;
    align-items: center;
    flex: 0 0 auto;
    flex-wrap: wrap;
    padding: 0.25rem;
    border-bottom: 1px solid #dcdfe6;
  }

  &__content {
    padding: 0.25rem 0.25rem;
    flex: 1 1 auto;
    overflow-x: hidden;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
  }
}
</style>

<style lang="scss">
/* Give a remote user a caret */
.collaboration-cursor__caret {
  position: relative;
  margin-left: -1px;
  margin-right: -1px;
  border-left: 1px solid #0d0d0d;
  border-right: 1px solid #0d0d0d;
  word-break: normal;
  pointer-events: none;
}

/* Render the username above the caret */
.collaboration-cursor__label {
  position: absolute;
  top: -1.4em;
  left: -1px;
  font-size: 12px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  user-select: none;
  color: #0d0d0d;
  padding: 0.1rem 0.3rem;
  border-radius: 3px 3px 3px 0;
  white-space: nowrap;
}

/* Basic editor styles */
.ProseMirror {
  height: 95%;
  > * + * {
    margin-top: 0.75rem;
  }
  line-height: normal;
  p {
    margin-top: 0;
  }
  a {
    cursor: pointer;
    color: #409eff;
    &:hover {
      text-decoration: underline;
    }
  }
  ul,
  ol {
    padding: 0 1rem;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    line-height: 1.1;
  }

  code {
    background-color: rgba(#616161, 0.1);
    color: #616161;
  }

  pre {
    background: #0d0d0d;
    color: #fff;
    font-family: 'JetBrainsMono', monospace;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;

    code {
      color: inherit;
      padding: 0;
      background: none;
      font-size: 0.8rem;
    }
  }

  mark {
    background-color: #faf594;
  }

  img {
    max-width: 100%;
    height: auto;
  }

  hr {
    margin: 1rem 0;
  }

  blockquote {
    padding-left: 1rem;
    border-left: 2px solid rgba(#0d0d0d, 0.1);
  }

  hr {
    border: none;
    border-top: 2px solid rgba(#0d0d0d, 0.1);
    margin: 2rem 0;
  }

  ul[data-type='taskList'] {
    list-style: none;
    padding: 0;

    li {
      display: flex;
      align-items: center;

      > label {
        flex: 0 0 auto;
        margin-right: 0.5rem;
      }
    }
  }
}
</style>
