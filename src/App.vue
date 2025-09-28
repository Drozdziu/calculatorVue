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
  <div class="container col-10 col-sm-8 col-md-6 col-lg-3">
    <DisplayComponent class="col-12" :calculations="{ digit: digit, refresh: refreshDisplay }" />
    <KeyBoard class="col-12" @digitEvent="digitEvent" />
  </div>
</template>

<style>
.container {
  margin-left: auto;
  margin-right: auto;
  margin-top: 10%;
  padding: 10px;
  background-color: rgb(27, 27, 29);
  min-width:400px;
  border: 5px solid black;
  border-radius: 10px;
}
</style>
