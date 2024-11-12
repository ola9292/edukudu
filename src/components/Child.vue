<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const emit = defineEmits(['change-text', 'toggle-bool']);
const props = defineProps({
  text: String,
  isActive: Boolean
});
const childText = ref(props.text);
const childIsActive = ref(props.isActive);

 const changeText = () => {
    emit('change-text', childText.value);
    };
    const toggleBool = () => {
        childIsActive.value = !childIsActive.value
        emit('toggle-bool', childIsActive.value);
    }
</script>
<template>
    <p>Child Component</p>
    <p>Bool:{{ childText }}</p>
    <p>Text:{{ childIsActive }}</p>
    <button @click="toggleBool">Change Bool</button><br /><br />
    <input type="text" v-model="childText" placeholder="name" @input="changeText">
    
</template>

<style>
    button{
        color:white;
        background-color: blue;
        border: none;
        padding: 4px 8px;
    }
</style>
