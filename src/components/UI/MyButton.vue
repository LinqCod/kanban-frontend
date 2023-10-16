<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'my-button'
    }
</script>

<style scoped>
    .btn {
        background-color: white;
        border: 2px solid black;
        padding: 5px 10px;
        border-radius: 5px;
    }
</style>