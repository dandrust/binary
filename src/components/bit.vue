<template>
    <div class="item binary-item" v-on:click="toggleState">{{ value }}</div>
</template>

<script>
    export default {
        props: ['value', 'editable'],
        computed: {
            readOnly: function () {
                return !this.editable;
            }
        },
        methods: {
            toggleState: function () {
                if (this.readOnly) {
                    return false;
                } else {
                    if (this.value === 0) {
                        this.$emit('input', 1);
                    } else {
                        this.$emit('input', 0);
                    }
                    this.$emit('recalculate');
                }
            },
        }
    }
</script>
