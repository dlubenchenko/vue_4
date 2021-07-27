<template>
    <button
        class="btn"
    >
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
    padding: 10px 15px;
    color: teal;
    border: 1px solid teal;
    background-color: #fff;
  }

  .btn:hover {
    border: 1px solid white;
    color: white;
    background-color: teal;
  }
</style>