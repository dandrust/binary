<template>
    <div>
        <BitMask v-for="(bitMask, index) in bitMasks"
                 v-bind:key="index"
                 v-bind:mode="mode"
                 v-bind:operator="bitMask.operator"
                 v-bind:order="order"
                 v-bind:value="bitMask.value"
                 v-on:input="updateBitMask(index, $event)"
                 v-on:remove="removeBitMask(index)" />

        <button v-on:click="addBitMask">Add bitmask</button>

        <p>Bitmask output:</p>
        <Binary v-if="anyBitMasksPresent"
                v-model="bitMaskOutput"
                v-bind:editable="false"
                v-bind:mode="mode"
                v-bind:order="order"
                v-bind:raw="false" />

    </div>
</template>

<script>
    import BitMask from './bit-mask'
    import Binary from './binary'

    export default {
        props: ['bitMasks', 'mode', 'order', 'anyBitMasksPresent', 'bitMaskOutput'],
        components: {
            Binary,
            BitMask
        },
        methods: {
            addBitMask: function () {
                this.$emit('add')
            },
            removeBitMask: function (index) {
                this.$emit('remove', index);
            },
            updateBitMask: function (index, value) {
                this.$emit('input', [index, value]);
            }
        }
    }
</script>