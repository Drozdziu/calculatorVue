<script lang="ts">
export default {
    name: 'Switch-dark-lightComponent',
    data(){
        return{
            mode: 'dark',
            isChecked: false
        }
    },
    watch:{
        isChecked(){
            const root = document.documentElement
            if(this.isChecked) root.setAttribute('data-theme', 'light')
            else root.removeAttribute('data-theme')
        }
    },
}
</script>
<template>
    <p>{{ isChecked ? "Light" : "Dark" }}</p>
    <label class="switch">
        <input type="checkbox" v-model="isChecked" ></input>
        <span class="slider"></span>
    </label>  
</template>

<style scoped>
p{
  text-align: center;
  color: var(--switchColor);
  font-weight: 900;
}
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #fffcfc;
  color: var(--buttonShadowColor);
  transition: .4s;
  border-radius: 10px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: #737379;
  transition: .4s;
  border-radius: 10px;
}

input:checked + .slider {
  background-color: #1a1b1a;
}

input:focus + .slider {
  box-shadow: 0 0 1px #fffcfc;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

</style>
