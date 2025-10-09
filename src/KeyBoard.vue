<script lang="ts">
import DigitComponent from './DigitComponent.vue';
export default {
  components: {
    DigitComponent
  },
  data() {
    return {
      signs: ['+', '-', '/', '*', '(', ')', 'X', 'C']
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
      <DigitComponent class="digits" sign="." @click="sendDigit('.')" />
      <DigitComponent class="digits" :digit="0" @click="sendDigit('0')" />
      <DigitComponent class="digits" sign="+/-" @click="sendDigit('+/-')" style="font-size: 20px;"/>
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
  background-color: var(--signFont-color);
  color: white;
  box-shadow: 0px 10px 0px rgb(160, 82, 18);
}

#equal:hover {
  background-color: white;
  color: var(--signFont-color);
  box-shadow: 0px 10px 0px rgb(189, 184, 181);
}

.signs {
  background-color: var(--buttonBG-color);
  color: var(--signFont-color);
}

.signs:hover {
  background-color: var(--signFont-color);
  color: var(--buttonBG-color);
  box-shadow: 0px 10px 0px rgb(138, 71, 17);
}

.digits {
  background-color: var(--buttonBG-color);
  color: var(--digitFont-color);
}

.digits:hover {
    background-color: var(--digitFont-color);
    color: var(--buttonBG-color);
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
</style>
