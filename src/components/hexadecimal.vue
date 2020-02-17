<template>
    <div>
        Hexadecimal Value: <code>{{ displayString }}</code><br>
        <div class="hexadecimal-container">
            <HexadecimalGroup v-for="(group, index) in groups" v-bind:key="index" v-bind:value="group.value" />
        </div>
    </div>

</template>

<script>
    import Vue from 'vue'
    import HexadecimalGroup from './hexidecimal-group'

    export default {
        components: {
            HexadecimalGroup,
        },
        props: ['value', 'mode', 'order'],
        data: function () {
            return {
                groups: {},
            }
        },
        computed: {
            hexadecimalString: function () {
                let hexString = (this.value >>> 0).toString(16);
                while (hexString.length < ( this.mode / 4) ) {
                    hexString = "0" + hexString;
                }
                return hexString;
            },
            displayString: function () {
                let hex = this.hexadecimalString;
                if (this.order === 'little-endian') {
                    hex = hex.split('').reverse().join('');
                }
                return '0x' + hex;
            }
        },
        methods: {
            updateGroups: function () {
                let groupCount = (this.mode / 8 );
                let fullHexadecimal = this.hexadecimalString.split('');
                for (let i = 0; i < groupCount; i++) {
                    Vue.set(this.groups, "group" + i, { value: fullHexadecimal.splice(-2, 2).join('') } );
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
                        Vue.delete(this.groups, 'group' + i);
                    }
                }
                this.updateGroups();
            },
            value: function () {
                this.updateGroups();
            }
        },
        created() {
            this.updateGroups();
        }
    }
</script>