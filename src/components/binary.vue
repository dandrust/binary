<template>
    <div>
        Binary Value: <code>{{ displayString }}</code><br>
        <div class="binary-container">
            <Byte v-for="(byte, index) in bytes" v-bind:key="index" v-model="byte.value" v-bind:editable="editable" v-on:recalculate="updateValue"/>
        </div>
    </div>

</template>

<script>
    import Vue from 'vue'
    import Byte from './byte'

    export default {
        components: {
            Byte,
        },
        props: ['value', 'mode', 'order', 'editable', 'raw'],
        data: function () {
            return {
                bytes: {},
            }
        },
        computed: {
            binaryString: function () {
                let binaryString;
                if (this.raw) {
                    binaryString = this.value;
                } else {
                    binaryString = (this.value >>> 0).toString(2);
                }

                while (binaryString.length < this.mode) {
                    binaryString = "0" + binaryString;
                }
                return binaryString;
            },
            displayString: function () {
                let binary = this.binaryString;
                if (this.order === 'little-endian') {
                    binary = binary.split('').reverse().join('');
                }
                return binary + 'b';
            },
            localBinaryValue: function () {
                return Object.values(this.bytes).reverse().reduce(function (string, byte) {
                    return string += String(byte.value);
                }, '')
            },
            localDecimalValue: function() {
                return parseInt(this.localBinaryValue, 2);
            }
        },
        methods: {
            updateBytes: function () {
                let byteCount = (this.mode / 8);
                let fullBinary = this.binaryString.split('');
                for (let i = 0; i < byteCount; i++) {
                    Vue.set(this.bytes, "byte" + i, { value: fullBinary.splice(-8, 8).join('') } );
                }
            },
            updateValue: function () {
                let updatedValue = this.raw ? this.localBinaryValue : this.localDecimalValue;
                this.$emit('input', updatedValue);
            }
        },
        watch: {
            mode: function (newMode, oldMode) {
                let oldModeInt = parseInt(oldMode);
                let newModeInt = parseInt(newMode);
                if ( newModeInt < oldModeInt ) {
                    let oldByteCount = oldModeInt / 8;
                    let newByteCount = newModeInt / 8;
                    for ( let i = newByteCount; i <= oldByteCount; i++ ){
                        Vue.delete(this.bytes, 'byte' + i);
                    }
                }
                this.updateBytes();
            },
            value: function () {
                this.updateBytes();
            }
        },
        created() {
            this.updateBytes();
        }
    }
</script>