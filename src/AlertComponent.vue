<script lang="ts">
export default {
  name: 'AlertComponent',
  props:{
    alert: String
  },
}
</script>

<template>
  <p>{{ alert }}</p>
</template>

<style scoped>
p{
    font-weight: 900;
    font-size: 50px;
    color: var(--switchColor);
    text-align: center;
}
</style>
