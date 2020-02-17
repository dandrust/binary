<template>
    <div class="hexadecimal-group-container">
        <HexadecimalValue v-for='digit in digits' v-bind:key='digit.exponent' v-model="digit.value"/>
    </div>
</template>

<script>
    import HexadecimalValue from './hexadecimal-value'

    export default {
        props: ['value'],
        components: {
            HexadecimalValue
        },
        data: function () {
            return {
                digits: {
                    digit0: { exponent: 0, value: null },
                    digit1: { exponent: 1, value: null },
                }
            }
        },
        computed: {
        },
        methods: {
            updateDigits: function () {
                this.digits.digit0.value = this.value[1].toUpperCase();
                this.digits.digit1.value = this.value[0].toUpperCase();
            }
        },
        watch: {
            value: function () {
                this.updateDigits();
            }
        },
        created() {
            this.updateDigits();
        }
    }
</script>