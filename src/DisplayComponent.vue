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
            zeroLimit: 0, // 0 - limit, 1 - bez limitu, 2 - zakaz
            openBracket: 0,
            closeBracket: 0,
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
                this.calculateString = this.calculateString.slice(0, -1);
            }
        }
    },
    watch: {
        calculations(newCalculate) {
            let lastSign = this.calculateString.charAt(this.calculateString.length - 1)
            switch (newCalculate.digit) {
                case '=':
                    if (this.openBracket == this.closeBracket && !this.calculateString.includes('/0')) {
                        try {
                            this.result = eval(this.calculateString);
                            this.calculateString = this.result.toFixed(2).toString();
                            this.openBracket = 0;
                            this.closeBracket = 0;
                            this.zeroLimit = 1;
                            this.alert = '';
                        } catch (e) {
                            this.alert = 'Something is wrong' + e
                            console.error(e);
                        }
                    } else this.alert = 'Something is wrong'
                    break;
                case 'C':
                    this.calculateString = '';
                    this.zeroLimit = 0;
                    this.openBracket = 0;
                    this.closeBracket = 0;
                    this.alert = '';
                    break;
                case 'X':
                    this.calculateString = this.calculateString.slice(0, -1);
                    break;
                case '(':
                    if (this.checkSign(lastSign) || this.calculateString == '') {
                        this.openBracket++;
                        this.write(newCalculate.digit)
                    } else this.alert = `You cannot open bracket here!`;
                    break;
                case ')':
                    if (!this.checkSign(lastSign) && this.openBracket >= this.closeBracket) {
                        this.closeBracket++;
                        this.write(newCalculate.digit)
                    } else this.alert = `Close brackets cannot be more than open brackets!`;
                    break;
                case '+':
                case '-':
                case '/':
                case '*':
                    if (this.calculateString != '' && !this.checkSign(lastSign)) {
                        this.write(newCalculate.digit)
                        this.zeroLimit = 0;
                    } else this.alert = `You cannot write ${newCalculate.digit} here!`
                    break;
                case '.':
                    if (this.calculateString != '' && !this.checkSign(lastSign)) {
                        this.write(newCalculate.digit)                  
                        this.zeroLimit = 1;
                    } else this.alert = `You cannot write ${newCalculate.digit} here!`
                    break;
                case '+/-':
                    if ((this.checkSign(lastSign) || this.calculateString == '') && !this.isMinus) {
                        this.isMinus = true;
                        this.write('(-');
                        this.openBracket++;
                    } else this.alert = `You cannot write (- here!`
                    break;
                case '√':
                    // if(this.checkSign(lastSign) || this.calculateString == ''){
                    //     console.log(this.openBracket)
                    //     // this.openBracket++;
                    //     this.write("Math.sqrt(");
                    // }
                    break;
                default:
                    if (newCalculate.digit === '0') {
                        if (this.zeroLimit == 0) {
                            this.zeroLimit = 2;
                            this.write(newCalculate.digit)
                        }
                        else if (this.zeroLimit == 1) this.write(newCalculate.digit)
                    }
                    if (newCalculate.digit !== '0') {
                        this.zeroLimit = 1;
                        if (this.calculateString == '0') this.calculateString = newCalculate.digit
                        else this.write(newCalculate.digit)
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
