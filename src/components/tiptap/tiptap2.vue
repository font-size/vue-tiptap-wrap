<template>
  <div class="ProseMirror">
    <div v-if="editor">
      <el-button :class="{ 'is-active': editor.isActive('bold') }">bold</el-button>
      <button @click="editor.chain().focus().toggleItalic().run()" :class="{ 'is-active': editor.isActive('italic') }">italic</button>
      <button @click="editor.chain().focus().toggleStrike().run()" :class="{ 'is-active': editor.isActive('strike') }">strike</button>
      <button @click="editor.chain().focus().toggleCode().run()" :class="{ 'is-active': editor.isActive('code') }">code</button>
      <button @click="editor.chain().focus().unsetAllMarks().run()">clear marks</button>
      <button @click="editor.chain().focus().clearNodes().run()">clear nodes</button>
      <button @click="editor.chain().focus().setParagraph().run()" :class="{ 'is-active': editor.isActive('paragraph') }">paragraph</button>
      <el-button @click="editor.chain().focus().toggleHeading({ level: 1 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }">
        h1
      </el-button>
      <button @click="editor.chain().focus().toggleHeading({ level: 2 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }">
        h2
      </button>
      <button @click="editor.chain().focus().toggleHeading({ level: 3 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }">
        h3
      </button>
      <button @click="editor.chain().focus().toggleHeading({ level: 4 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }">
        h4
      </button>
      <button @click="editor.chain().focus().toggleHeading({ level: 5 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }">
        h5
      </button>
      <button @click="editor.chain().focus().toggleHeading({ level: 6 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }">
        h6
      </button>
      <button @click="editor.chain().focus().toggleBulletList().run()" :class="{ 'is-active': editor.isActive('bulletList') }">bullet list</button>
      <button @click="editor.chain().focus().toggleOrderedList().run()" :class="{ 'is-active': editor.isActive('orderedList') }">ordered list</button>
      <button @click="editor.chain().focus().toggleCodeBlock().run()" :class="{ 'is-active': editor.isActive('codeBlock') }">code block</button>
      <button @click="editor.chain().focus().toggleBlockquote().run()" :class="{ 'is-active': editor.isActive('blockquote') }">blockquote</button>
      <button @click="editor.chain().focus().setHorizontalRule().run()">horizontal rule</button>
      <button @click="editor.chain().focus().setHardBreak().run()">hard break</button>
      <button @click="editor.chain().focus().undo().run()">undo</button>
      <button @click="editor.chain().focus().redo().run()">redo</button>
    </div>
    <editor-content :editor="editor" />
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2';
import StarterKit from '@tiptap/starter-kit';

export default {
  components: {
    EditorContent,
  },

  props: {
    value: {
      type: String,
      default: '',
    },
  },

  data() {
    return {
      editor: null,
    };
  },

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
    this.editor = new Editor({
      extensions: [StarterKit],
      content: this.value,
      onUpdate: () => {
        // HTML
        this.$emit('input', this.editor.getHTML());

        // JSON
        // this.$emit('input', this.editor.getJSON())
      },
    });
  },

  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>
<style scoped lang="scss">
/* Basic editor styles */
.ProseMirror {
  > * + * {
    margin-top: 0.75em;
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

  img {
    max-width: 100%;
    height: auto;
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
}
</style>
