<template>
    <div>
        Binary Value: <code>{{ displayString }}</code><br>
        <div class="binary-container">
            <Byte v-for="(byte, index) in bytes" v-bind:key="index" v-bind:value="byte.value" />
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
        props: ['value', 'mode', 'order'],
        data: function () {
            return {
                bytes: {},
            }
        },
        computed: {
            binaryString: function () {
                let binaryString = (this.value >>> 0).toString(2);
                while (binaryString.length < this.modeInt) {
                    binaryString = "0" + binaryString;
                }
                return binaryString;
            },
            modeInt: function () {
                return parseInt(this.mode);
            },
            displayString: function () {
                let binary = this.binaryString;
                if (this.order === 'little-endian') {
                    binary = binary.split('').reverse().join('');
                }
                return binary + 'b';
            }
        },
        methods: {
            updateBytes: function () {
                let byteCount = (this.modeInt / 8);
                let fullBinary = this.binaryString.split('');
                for (let i = 0; i < byteCount; i++) {
                    Vue.set(this.bytes, "byte" + i, { value: fullBinary.splice(-8, 8).join('') } );
                }
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