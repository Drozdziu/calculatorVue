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
            isMinus: false,
            isDot: false,
            zeroLimit: 0, // 0 - limit, 1 - bez limitu, 2 - zakaz
            signs: ['+', '-', '/', '*', '.', '('],
            alert: '',
        }
    },
    methods: {
        checkSign(sign: string) {
            let bool = false;
            this.signs.forEach((i) => {
                if (sign === i) bool = true;
            })
            return bool;
        },
        write(str: string) {
            this.calculateString += str;
            if (this.alert != '') {
                this.alert = '';
                if(this.calculateString.slice(-1) !== '-') this.calculateString = this.calculateString.slice(0, -1);
            }
        }
    },
    watch: {
        calculations(newCalculate) {
            let lastSign = this.calculateString.charAt(this.calculateString.length - 1);
            let openBracket = this.calculateString.split('(').length - 1;
            let closeBracket = this.calculateString.split(')').length - 1;
            switch (newCalculate.digit) {
                case '=':
                    if (!this.calculateString.includes('/0') || this.calculateString.includes('/0.') && !this.calculateString.includes('√((-') && openBracket == closeBracket) {
                        try {
                            this.calculateString = this.calculateString.replace(/√/g, 'Math.sqrt') //replaceAll('√', 'Math.sqrt');
                            this.result = eval(this.calculateString);
                            if(Number.isInteger(this.result)){
                                this.calculateString = this.result.toString();
                                this.isDot = false;
                            } 
                            else{
                                this.calculateString = parseFloat(this.result.toFixed(10).toString()).toString();
                                this.isDot = true;
                            } 
                            this.zeroLimit = 1;
                            this.alert = '';
                            this.isMinus = false;
                        } catch (e) {
                            this.alert = 'Something is wrong ' + e;
                            console.error(e);
                        }
                    } else{
                        if(this.calculateString.includes('/0')) this.alert = 'You cannot divide by 0!';
                        if(this.calculateString.includes('√((-')) this.alert = 'Squre root cannot be negative!';
                        if(openBracket != closeBracket) this.alert = 'Some bracket is not closed!';
                    } 
                    break;
                case 'C':
                    this.calculateString = '';
                    this.zeroLimit = 0;
                    this.isDot = false;
                    this.alert = '';
                    break;
                case '⌫':
                    this.calculateString = this.calculateString.slice(0, -1);
                    break;
                case '(':
                    if (this.checkSign(lastSign) || this.calculateString == '') this.write(newCalculate.digit);
                    else this.alert = `You cannot open bracket here!`;
                    break;
                case ')':
                    if (!this.checkSign(lastSign) && openBracket > closeBracket) this.write(newCalculate.digit);
                    else this.alert = `Close brackets cannot be more than open brackets!`;
                    break;
                case '+':
                case '-':
                case '/':
                case '*':
                    if (this.calculateString != '' && !this.checkSign(lastSign)) {
                        this.write(newCalculate.digit);
                        this.zeroLimit = 0;
                        this.isDot = false;
                        this.isMinus = false;
                    } else this.alert = `You cannot write ${newCalculate.digit} here!`
                    break;
                case '.':
                    if (this.calculateString != '' && !this.checkSign(lastSign) && !this.isDot) {
                        this.write(newCalculate.digit)      ;            
                        this.zeroLimit = 1;
                        this.isDot = true;
                    } else this.alert = `You cannot write ${newCalculate.digit} here!`
                    break;
                case '+/-':
                    if ((this.checkSign(lastSign) || this.calculateString == '') && !this.isMinus) {
                        this.isMinus = true;
                        this.write('(-');
                    } else this.alert = `You cannot write (- here!`
                    break;
                case '√':
                    if(this.checkSign(lastSign) || this.calculateString == '') this.write("√(");
                    break;
                default:
                    if (newCalculate.digit === '0') {
                        if (this.zeroLimit == 0) {
                            this.zeroLimit = 2;
                            this.write(newCalculate.digit);
                        }
                        else if (this.zeroLimit == 1) this.write(newCalculate.digit);
                    }
                    if (newCalculate.digit !== '0') {
                        this.zeroLimit = 1;
                        if (this.calculateString == '0') this.calculateString = newCalculate.digit;
                        else this.write(newCalculate.digit);
                    }
                    break;
            }
        },
        alert() { this.$emit('alertEvent', this.alert); }
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
    background-color: var(--buttonBG-color);
    color: black;
    text-align: center;
    box-shadow: 0px 5px 0px var(--buttonShadowColor);
}
</style>
