<template>
  <div class="editor-container">
    <Toolbar @format="applyFormat" @insertImage="insertImage" @copyHtml="copyHtml" @undo="undo" @redo="redo" />
    <div ref="editor" contenteditable="true" class="editor" @input="saveToHistory" @paste="handlePaste"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import Toolbar from './Toolbar.vue';

const editor = ref<HTMLElement | null>(null);
const history: string[] = [];
let historyIndex = -1;

const applyFormat = (command: string, value?: string) => {
  document.execCommand(command, false, value || '');
  saveToHistory();
};

const insertImage = () => {
  const url = prompt('Введите URL изображения');
  if (url) {
    document.execCommand('insertImage', false, url);
    saveToHistory();
  }
};

const copyHtml = () => {
  if (editor.value) {
    navigator.clipboard.writeText(editor.value.innerHTML);
  }
};

const saveToHistory = () => {
  if (editor.value) {
    history.push(editor.value.innerHTML);
    historyIndex++;
    localStorage.setItem('editorContent', editor.value.innerHTML);
  }
};

const handlePaste = (event: ClipboardEvent) => {
  event.preventDefault();
  const text = event.clipboardData?.getData('text/plain');
  if (text) document.execCommand('insertText', false, text);
  saveToHistory();
};

const undo = () => {
  if (historyIndex > 0) {
    historyIndex--;
    editor.value!.innerHTML = history[historyIndex];
    localStorage.setItem('editorContent', editor.value!.innerHTML);
  }
};

const redo = () => {
  if (historyIndex < history.length - 1) {
    historyIndex++;
    editor.value!.innerHTML = history[historyIndex];
    localStorage.setItem('editorContent', editor.value!.innerHTML);
  }
};

onMounted(() => {
  if (localStorage.getItem('editorContent')) {
    editor.value!.innerHTML = localStorage.getItem('editorContent')!;
  }
  nextTick(() => {
    editor.value?.focus();
  });
});
</script>
