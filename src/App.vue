<script lang="ts">
import KeyBoard from './KeyBoard.vue';
import DisplayComponent from './DisplayComponent.vue';
import DigitComponent from './DigitComponent.vue';
import SwitchDarkLight from './SwitchDarkLight.vue';
import AlertComponent from './AlertComponent.vue';
export default {
  components: {
    KeyBoard,
    DisplayComponent,
    DigitComponent,
    SwitchDarkLight,
    AlertComponent
  },
  data() {
    return {
      digit: '',
      alert: '',
      equals: false,
      refreshDisplay: 0
    }
  },
  methods: {
    digitEvent(mess: string) {
      this.digit = mess;
      this.refreshDisplay++;
    },
    alertEvent(alert: string){
      this.alert = alert;
    }
  }
} 
</script>

<template>
  <main>
    <div id="darkLight">
      <SwitchDarkLight />
    </div>
    <div class="container col-10 col-sm-8 col-md-6 col-lg-3">
      <DisplayComponent class="col-12" @alertEvent="alertEvent" :calculations="{ digit: digit, refresh: refreshDisplay }" />
      <KeyBoard class="col-12" @digitEvent="digitEvent" />
    </div>
    <AlertComponent :alert="alert"/>
  </main>
</template>

<style>
main {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  grid-template-rows: repeat(1, 1fr);
}

.container {
  margin-top: 10%;
  padding: 10px;
  background-color: var(--BGcolor);
  min-width: 400px;
  border: 5px solid black;
  border-radius: 10px;
}

#darkLight {
  position: relative;
  top: 30%;
  margin-left: auto;
  margin-right: auto;
}
</style>
