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
    transition: 100ms;
    border-width: 0px;
    box-shadow: 0px 10px 0px rgb(75, 75, 80);
}
/* button::after{
    position: absolute;
    content: '';
    background: rgb(75, 75, 80);
    height: 20px;
    width: 100%;
    border-radius: 10px;
    margin-bottom: -5px;
    left: 0;
    bottom: 0;
    z-index: -1;
} */
</style>
