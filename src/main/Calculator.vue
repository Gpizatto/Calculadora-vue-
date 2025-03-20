<template>
  <div class="calculator">
    <DisplayComponent :value="displayValue" />
    <ButtonComponent label="AC" triple @onCLick="clearMemory"/>
    <ButtonComponent label="/" operatio  @onCLick="setOperation"/>
    <ButtonComponent label="7"  @onCLick="addDigito"/>
    <ButtonComponent label="8" @onCLick="addDigito"/>
    <ButtonComponent label="9" @onCLick="addDigito"/>
    <ButtonComponent label="*" operation @onCLick="setOperation"/>
    <ButtonComponent label="4" @onCLick="addDigito"/>
    <ButtonComponent label="5" @onCLick="addDigito"/>
    <ButtonComponent label="6" @onCLick="addDigito"/>
    <ButtonComponent label="-" operation/>
    <ButtonComponent label="1" @onCLick="addDigito"/>
    <ButtonComponent label="2" @onCLick="addDigito"/>
    <ButtonComponent label="3" @onCLick="addDigito"/>
    <ButtonComponent label="+" operation @onCLick="setOperation"/>
    <ButtonComponent label="0" double  @onCLick="addDigito"/>
    <ButtonComponent label="." @onCLick="addDigito"/>
    <ButtonComponent label="=" operation @onCLick="setOperation"/>


  
    
  </div>
</template>

<script>
import ButtonComponent from"../components/Button.vue"
import DisplayComponent from"../components/Display"


export default {
   data: function() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0
        }
    },
  
  name:"AppComponent",

    components: { ButtonComponent,DisplayComponent},
    methods:{
        clearMemory() {
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {
            if (this.current === 0) {
                this.operation = operation
                this.current = 1
                this.clearDisplay = true
            } else {
                const equals = operation === "="
                const currentOperation = this.operation

                try {
                    this.values[0] = eval(
                        `${this.values[0]} ${currentOperation} ${this.values[1]}`
                    )
                } catch (e) {
                    this.$emit('onError', e)
                }

                this.values[1] = 0

                this.displayValue = this.values[0]
                this.operation = equals ? null : operation
                this.current = equals ? 0 : 1
                this.clearDisplay = !equals
            }
        },
        addDigit(n) {
            if (n === "." && this.displayValue.includes(".")) {
                return
            }

            const clearDisplay = this.displayValue === "0"
                || this.clearDisplay
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n

            this.displayValue = displayValue
            this.clearDisplay = false
            
            // Alternativa 1
            this.values[this.current] = displayValue
            
            // Alternativa 2
            // if (n !== ".") {
            //     const i = this.current
            //     const newValue = parseFloat(displayValue)
            //     this.values[i] = newValue
            // }
        }
    }
}
</script>


<style>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>
