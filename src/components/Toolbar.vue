<template>
  <div class="toolbar">
    <button @click="$emit('format', 'bold')"><strong>B</strong></button>
    <button @click="$emit('format', 'italic')"><em>I</em></button>
    <button @click="$emit('format', 'underline')"><u>U</u></button>
    <button @click="$emit('format', 'insertUnorderedList')">• List</button>
    <button @click="$emit('format', 'formatBlock', 'h2')">H2</button>
    <button @click="$emit('format', 'formatBlock', 'p')">P</button>
    <button @click="$emit('insertImage')">🖼️ Img</button>
    <button @click="$emit('copyHtml')">📋 Copy HTML</button>
    <button @click="$emit('undo')">↩️ Undo</button>
    <button @click="$emit('redo')">↪️ Redo</button>
  </div>
</template>

<script setup>
defineEmits(['format', 'insertImage', 'copyHtml', 'undo', 'redo']);
</script>
