<template>
    <div class="byte-container">
        <Bit v-for='bit in bits' v-bind:key='bit.exponent' v-model="bit.value"/>
    </div>
</template>

<script>
    import Bit from './bit'

    export default {
        components: {
            Bit
        },
        props: ['value'],
        data: function () {
            return {
                bits: {
                    bit0: { exponent: 0, value: null },
                    bit1: { exponent: 1, value: null },
                    bit2: { exponent: 2, value: null },
                    bit3: { exponent: 3, value: null },
                    bit4: { exponent: 4, value: null },
                    bit5: { exponent: 5, value: null },
                    bit6: { exponent: 6, value: null },
                    bit7: { exponent: 7, value: null }
                }
            }
        },
        computed: {
            decimal: function () {
                return Object.values(this.bits).reduce(function (sum, bit) {
                    return sum + (bit.value * (2 ** bit.exponent));
                }, 0)
            },
        },
        methods: {
            updateBits: function () {
                this.bits.bit0.value = parseInt(this.value[7]);
                this.bits.bit1.value = parseInt(this.value[6]);
                this.bits.bit2.value = parseInt(this.value[5]);
                this.bits.bit3.value = parseInt(this.value[4]);
                this.bits.bit4.value = parseInt(this.value[3]);
                this.bits.bit5.value = parseInt(this.value[2]);
                this.bits.bit6.value = parseInt(this.value[1]);
                this.bits.bit7.value = parseInt(this.value[0]);
            }
        },
        watch: {
            value: function () {
                this.updateBits();
            }
        },
        created() {
            this.updateBits()
        }
    }
</script>