<template>
    <div style="background-color: lightgray">
        Operator: <button v-on:click="cycleOperator">{{ this.operator }}</button>
        <button v-on:click="removeBitMask">Remove</button>
        <Binary v-model="localValue" v-bind:mode="mode" v-bind:order="order" v-bind:editable="true" v-bind:raw="true" v-on:input="updateValue"/>
    </div>
</template>

<script>
    var bitMaskOperators = [
        'and',
        'or',
        'xor'
    ];

    import Binary from './binary'

    export default {
        props: ['value', 'operator', 'mode', 'order'],
        components: {
            Binary
        },
        data: function () {
            return {
                localValue: this.value,
                localOperator: this.operator
            }
        },
        methods: {
            updateValue: function () {
                this.$emit('input', { value: this.localValue, operator: this.localOperator })
            },
            cycleOperator: function () {
                let currentIndex = bitMaskOperators.indexOf(this.localOperator);
                let lastIndex = bitMaskOperators.length - 1;
                let nextIndex = currentIndex === lastIndex ? 0 : (currentIndex + 1);
                this.localOperator = bitMaskOperators[nextIndex];
                this.updateValue();
            },
            removeBitMask: function () {
                this.$emit('remove');
            }
        }
    }
</script>
