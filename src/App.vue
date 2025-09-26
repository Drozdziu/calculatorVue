<script lang="ts">
import KeyBoard from './KeyBoard.vue';
import DisplayComponent from './DisplayComponent.vue';
import DigitComponent from './DigitComponent.vue';
export default {
  components: {
    KeyBoard,
    DisplayComponent,
    DigitComponent
  },
  data(){
    return{
      digit: '',
      equals: false,
      refreshDisplay: 0
    }
  },
  methods: {
    digitEvent(mess: string){
      this.digit = mess;
      this.refreshDisplay++;
    },
  }
} 
</script>

<template>
  <div class="container">
    <DisplayComponent :calculations="{ digit: digit, refresh: refreshDisplay }" />
    <KeyBoard @digitEvent="digitEvent" />
  </div>
</template>

<style scoped>
.container {
  margin-left: auto;
  margin-right: auto;
  margin-top: 10%;
  padding: 10px;
  background-color: black;
  max-width: 500px;
  border: 5px solid black;
  border-radius: 10px;
}
</style>
