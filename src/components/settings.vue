<template>
    <div>
        <button v-on:click="toggleState"><span v-if="state">Hide </span>Settings</button>
        <div v-if="state">
            <h1>Settings</h1>
            <h2>Mode</h2>
            <input type="radio" value="1" v-model="value.modeIndex">
            <label>8 bit</label><br>
            <input type="radio" value="2" v-model="value.modeIndex">
            <label>16 bit</label><br>
            <input type="radio" value="3" v-model="value.modeIndex">
            <label>24 bit</label><br>
            <input type="radio" value="4" v-model="value.modeIndex">
            <label>32 bit</label><br>

            <h2>Bit Order</h2>
            <input type="radio" value="big-endian" v-model="value.bitOrder">
            <label>Big-endian</label><br>
            <input type="radio" value="little-endian" v-model="value.bitOrder">
            <label>Little-endian</label><br>

            <h2>Hexadecimal</h2>
            <input type="checkbox" v-model="value.showHexadecimal">
            <label>Show hexadecimal values</label>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['value'],
        data: function () {
            return {
                state: false
            }
        },
        methods: {
            toggleState: function () {
                this.state = !this.state;
            }
        }
    }
</script>