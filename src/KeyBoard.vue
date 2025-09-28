<script lang="ts">
import DigitComponent from './DigitComponent.vue';
export default {
  components: {
    DigitComponent
  },
  data() {
    return {
      signs: ['+', '-', '/', '*', '(', ')', '.', 'X', 'C']
    }
  },
  methods: {
    sendDigit(n: string) {
      this.$emit('digitEvent', n)
    },
  }
}
</script>

<template>
  <main>
    <div class="keyboard">
      <DigitComponent class="digits" v-for="n in 9" :key="n" :digit="n" @click="sendDigit(n.toString())" />
      <DigitComponent class="digits" :digit="0" @click="sendDigit('0')" />
    </div>
    <div class="keyboard">
      <DigitComponent class="signs" v-for="i in signs" :sign="i" @click="sendDigit(i)" />
      <DigitComponent id="equal" sign="=" @click="sendDigit('=')" />
    </div>
  </main>

</template>

<style scoped>
main {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(1, 1fr);
}

#equal {
  background-color: rgb(209, 102, 15);
  color: white;
  box-shadow: 0px 10px 0px rgb(160, 82, 18);
}

#equal:hover {
  background-color: white;
  color: rgb(209, 102, 15);
  box-shadow: 0px 10px 0px rgb(189, 184, 181);
}

.signs {
  background-color: rgb(91, 91, 94);
  color: rgb(209, 102, 15);
}

.signs:hover {
  background-color: rgb(209, 102, 15);
  color: rgb(91, 91, 94);
  box-shadow: 0px 10px 0px rgb(138, 71, 17);
}

.digits {
  background-color: rgb(91, 91, 94);
  color: rgb(46, 221, 11);
}

.digits:hover {
    background-color: rgb(46, 221, 11);
    color: rgb(91, 91, 94);
    box-shadow: 0px 10px 0px rgb(43, 153, 21);
}

.digits:active, .signs:active, #equal:active{
  margin-bottom: 5px;
  box-shadow: 0px 5px 0px;
}

.keyboard {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 1fr);
  gap: 1px;
  border-radius: 10px;
}
.keyboard > :last-child  {
  grid-column: 2/ 2; 
}
.keyboard #equal  {
  grid-column: 1 / -1; 
}
</style>
