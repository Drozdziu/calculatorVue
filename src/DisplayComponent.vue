<script lang="ts">
export default {
    name: 'DisplayComponent',
    props: {
        calculations: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            calculateString: '',
            result: 0,
            isSign: true,
            isDot: false,
            zeroLimit: 0, // 0 - limit, 1 - bez limitu, 2 - zakaz
            openBracket: 0,
            closeBracket: 0,
            signs: ['+', '-', '/', '*', '.', '(']
        }
    },
    methods: {
        checkSign(sign: string) {
            let bool = false;
            this.signs.forEach((i) => {
                if (sign === i && sign !== '-') bool = true;
            })
            return bool;
        },
        write(str: string){
            this.calculateString += str;
            this.isSign = false;
        }
    },
    watch: {
        calculations(newCalculate) {
            let lastSign = this.calculateString.charAt(this.calculateString.length - 1)
            switch (newCalculate.digit) {
                case '=':
                    if (this.openBracket == this.closeBracket) {
                        this.result = eval(this.calculateString);
                        this.calculateString = this.result.toFixed(2).toString();
                        this.isDot = true;
                        this.openBracket = 0;
                        this.closeBracket = 0;
                        this.zeroLimit = 1;
                    }
                    break;
                case 'C':
                    this.calculateString = '';
                    this.isDot = false;
                    this.isSign = true;
                    this.zeroLimit = 0;
                    break;
                case 'X':
                    if (lastSign === '.'){
                        this.isDot = false;
                        this.zeroLimit = 0;
                    } 
                    if (!this.checkSign(lastSign)){
                        this.isSign = false;
                    } 
                    this.calculateString = this.calculateString.slice(0, -1);
                    break;
                case '(':
                    this.openBracket++;
                    this.write(newCalculate.digit)
                    break;
                case ')':
                    if (!this.checkSign(lastSign)) {
                        this.closeBracket++;
                        this.write(newCalculate.digit)
                    }
                    break;
                case '+':
                case '-':
                case '/':
                case '*':
                    if (!this.isSign && this.calculateString != '') {
                        this.write(newCalculate.digit)
                        this.isSign = true;
                        this.isDot = false;
                        this.zeroLimit = 0;
                    }
                    break;
                case '.':
                    if (!this.isDot && this.calculateString != '' && !this.checkSign(lastSign)) {
                        this.write(newCalculate.digit)
                        this.isDot = true;
                        this.isSign = true;
                        this.zeroLimit = 1;
                    }
                    break;
                default:
                    if (newCalculate.digit === '0'){ 
                        if(this.zeroLimit == 0){
                            this.zeroLimit = 2;
                            this.write(newCalculate.digit)
                        }
                        else if(this.zeroLimit == 1) this.write(newCalculate.digit)
                    }
                    if (newCalculate.digit !== '0'){
                        this.zeroLimit = 1;
                        if(this.calculateString.length == 1 && this.calculateString == '0') this.calculateString = newCalculate.digit
                        else this.write(newCalculate.digit)
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
    font-weight: 900;
    border-radius: 10px;
    background-color: rgb(91, 91, 94);
    color: black;
    text-align: center;
    box-shadow: 0px 5px 0px rgb(75, 75, 80);
}
</style>
