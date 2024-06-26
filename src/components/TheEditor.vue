<script setup>
import { EditorContent } from '@tiptap/vue-3'
import { onMounted, onBeforeUnmount } from 'vue'
import { useCommandStore } from '@/stores/commands'
import { useEditorStore } from '@/stores/editor'
import TheCommandPalette from '@/components/TheCommandPalette.vue'
import { logUserAction } from '@/assets/js/logging.js'

/*
    >>>> MENUS
    https://tiptap.dev/docs/editor/guide/menus

    > Bubble Menu
    – A bubble menu is one that appears when selecting text.
    – https://tiptap.dev/docs/editor/api/extensions/bubble-menu

    > Floating Menu
    – A floating menu appears in the editor when you place the cursor on an new line.
    - Docs: https://tiptap.dev/docs/editor/api/extensions/bubble-menu
*/
const editorStore = useEditorStore()
const commandStore = useCommandStore()

// const customPrompt = ref('')
// const startIndex = ref(0)

onMounted(async () => {
  editorStore.createEditor()
  commandStore.initTemplate(editorStore.editor)
})

onBeforeUnmount(() => {
  editorStore.editor.destroy()
})

// async function run(editor, prompt) {
//   commandStore.run(editor, prompt, prompt.startIndex ?? 0)
//   startIndex.value = prompt.startIndex + 1

//   // Flat prompt after generate options
//   if (prompt.actions[prompt.startIndex ?? 0].type === 'generate options') {
//     prompt.generateOptionsFlag = true
//   }
// }

// function onCustomPrompt(editor, trigger = 'new-line', mode = 'append') {
//   let customPromptObj = {
//     name: 'continue',
//     trigger: trigger,
//     mode: mode,
//     actions: [
//       {
//         type: 'generate',
//         template:
//           'Considering the following story, which is delimited with triple backticks, perform the following task. \n\nTask: ' +
//           customPrompt.value +
//           '. \n\nGenerate one short sentence, using at most 10 words. Write in a narrative way, keeping the tone and style of the story. \n\nStory: ```::full::```'
//       }
//     ]
//   }
//   commandStore.run(editor, customPromptObj, 0)
// }

// function onShowModal() {
//   customPrompt.value = ''
// }

// function onKeyDown(e) {
//   console.log(e, 'keydown')
// }
</script>

<template>
  <editor-content :editor="editorStore.editor" class="editor" />
  <TheCommandPalette />
</template>

<style scoped>
.editor {
  margin-top: 50vh;
  margin-bottom: 50vh;
  grid-column: center-start / center-end;
  grid-row: center-start / center-end;

  font: var(--font-text);
  letter-spacing: 0.5px;
  color: var(--color-user);

  caret-color: var(--color-user);

  @media print {
    margin-top: calc(var(--spacing) * 1.5);
    margin-bottom: calc(var(--spacing) * 1.5);
  }
}

.editor:deep(> div:focus) {
  outline: none;
}

.editor:deep(*::selection) {
  background-color: color-mix(in lab, var(--color-user), transparent 85%);
}

.editor:deep(span.mark-ai) {
  color: var(--color-ai);
}

.editor:deep(span.mark-ai::selection) {
  background-color: color-mix(in lab, var(--color-ai), transparent 85%);
}

.editor:deep(p + p) {
  margin-top: calc(var(--font-size) * var(--line-height));
}

.editor:deep(h1, h2, h3, h4) {
  font: var(--font-heading);
  letter-spacing: 0;
}

.editor:deep([data-type='placeholder']) {
  font: var(--font-heading);
  font-weight: 425;
  font-size: 22px;
  color: var(--color-user);
  background: color-mix(in lab, var(--color-user), transparent 85%);
  border-radius: 5px;
  padding: 0 calc(var(--spacing) / 4);
  box-decoration-break: clone;
}

button {
  text-align: left;
}

div:has(> button.diverge) {
  display: grid;
  grid-auto-rows: 1fr;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 0.3em;
  grid-row-gap: 0.3em;
}
button.diverge {
  padding: 14px 18px;
  display: inline-flex;
}

input {
  width: 100%;
  display: block;
  margin-top: 10px;
}

.is-generating {
  grid-column: center-start;
  grid-row: center-end;
  color: #999999;
}
</style>
