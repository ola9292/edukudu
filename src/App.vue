<script setup>
  import Child from './components/Child.vue';
  import { ref, watch } from 'vue';
  import less from 'less';
  //
  let isActive = ref(false);
  let text = ref('olamide');

  //preview elements
  let htmlCode = ref('<p>Hello, world!</p>')
  let lessCode = ref('p { color: blue; }')

  const previewFrame = ref(null);

  const changeText = (value) => {
    text.value = value
  }
  const toggleBool = (value) => {
    isActive.value = value
  }

  const compileLess = async () => {
  try {
    const output = await less.render(lessCode.value);
    updatePreview(output.css);
  } catch (error) {
    console.error("Error compiling LESS:", error);
  }
};

// Function to update the preview in the iframe
const updatePreview = (compiledCSS) => {
  const doc = previewFrame.value.contentDocument;
  doc.open();
  doc.write(`
    <style>${compiledCSS}</style>
    ${htmlCode.value}
  `);
  doc.close();
};

// Watchers to update the preview whenever HTML or compiled LESS changes
watch([htmlCode, lessCode], compileLess, { immediate: true });
</script>


<template>
      <h2>Parent Child Component</h2>
    <div class="container">
      
      <p>Parent Component</p>
      <p>Bool:{{ isActive }}</p>
      <p>Text:{{ text }}</p>
      <div class="child-container">
        <Child :text="text" :isActive="isActive" @change-text="changeText" @toggle-bool="toggleBool"/>
      </div>
    </div>
    <div class="preview-editor">
      <h2>preview editor</h2>
      <div class="textarea-container">
        <textarea class="textarea-one" v-model="htmlCode"  placeholder="type HTML here" height="400"></textarea>
        <textarea class="textarea-two" v-model="lessCode"  placeholder="type LESS here" height="400"></textarea>
      </div>
      
      <iframe ref="previewFrame" src="" frameborder="1"></iframe>
    </div>
</template>

<style>
  .container{
    background-color: rgb(134, 144, 90);
    padding: 20px;
  }
  .child-container{
    background-color: gray;
    padding: 10px;
  }
  h2{
    text-align: center;
    text-transform: capitalize;
  }
  .textarea-container{
    display:flex;
    gap:10px;
    margin-bottom: 10px;
  }
  textarea{
    flex: 1;
  }
  iframe{
    width: 100%;
    height: 300px;
  }
</style>
