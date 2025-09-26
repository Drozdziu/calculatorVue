<script lang="ts">
export default {
    name: 'DigitComponent',
    props: {
        digit: Number,
        sign: String
    },
}
</script>
<template>
    <button>{{ digit == null ? sign : digit }}</button>
</template>

<style scoped>
button {
    border-radius: 10px;
    margin: 10px;
    padding: 10px;
    font-weight: 900;
    font-size: 30px;
    transition: 200ms;
    border: 1px solid white;
}

button:hover {
    background-color: rgb(46, 221, 11);
    color: rgb(91, 91, 94);
}
</style>
