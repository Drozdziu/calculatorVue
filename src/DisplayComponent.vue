<script lang="ts">
export default {
    name: 'DisplayComponent',
    props: {
        calculations:{
            type: Object,
            required: true
        }
    },
    data(){
        return{
            calculateString: '',
            result: 0,
            isSign: true,
            isDot: false,
            signs: ['+', '-', '/', '*', '.']
        }
    },
    methods:{
        checkSign(sign: string){
            let bool = false;
            this.signs.forEach((i)=>{ 
                if(sign === i) bool = true; 
            })
            return bool;
        }
    },
    watch: {
        calculations(newCalculate){
            let lastSign = this.calculateString.charAt(this.calculateString.length - 1)
            switch(newCalculate.digit){
                case '=':
                    this.result = eval(this.calculateString);
                    this.calculateString = this.result.toFixed(2).toString();
                    break;
                case 'C':
                    this.calculateString = '';
                    this.isDot = false;
                    this.isSign = true;
                    break;
                case 'X':
                    if(lastSign === '.') this.isDot = false;
                    if(!this.checkSign(lastSign)) this.isSign = false;
                    this.calculateString = this.calculateString.slice(0, -1);
                    break;
                case '+':
                case '-':
                case '/':
                case '*':
                    if(!this.isSign && this.calculateString != ''){
                        this.calculateString += newCalculate.digit;
                        this.isSign = true;
                        this.isDot = false;
                    } 
                    break;
                case '.':
                     if(!this.isDot && this.calculateString != '' && !this.checkSign(lastSign)){
                        this.calculateString += newCalculate.digit;
                        this.isDot = true;
                        this.isSign = true;
                     }
                    break;
                default:
                    if(newCalculate.digit === '0' && (this.calculateString === '' || this.checkSign(lastSign))) return
                    else{
                        this.calculateString += newCalculate.digit;
                        this.isSign = false;
                    }
                    break;
            }
        }
    }
}

</script>

<template>
    <h1>{{ calculateString }}</h1>
</template>

<style scoped>
h1 {
    min-height: 50px;
    border: 1px solid white;
    border-radius: 10px;
    background-color: rgb(91, 91, 94);
    color: black;
    text-align: center;
}
</style>
