<template>
    <input 
        class="input"
        type="text"
        :value="modelValue"
        @input="updateInput"
    >
</template>

<script>
    export default {
        name: 'my-input',
        props: {
            modelValue: [String, Number] 
        },
        methods: {
            updateInput(event) {
                this.$emit('update:modelValue', event.target.value);
            }
        }
    }
</script>

<style scoped>
     .input {
        background: none;
        border: 2px solid black;
        border-radius: 5px;
        padding: 5px 10px;
    }
</style>