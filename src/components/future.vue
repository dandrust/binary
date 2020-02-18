<template>
    <div>
        <button v-on:click="toggleState"><span v-if="state">Hide </span>Hopes and Dreams</button>
        <div v-if="state">
            <h1>Hopes and Dreams</h1>
            <ul>
                <li>Add bitmasks and logical operators at runtime</li>
                <li>Visualize ASCII character codes, escape codes, and bitmasks <a href="https://viewsourcecode.org/snaptoken/kilo/03.rawInputAndOutput.html#press-ctrl-q-to-quit" target="_blank">as discussed here</a></li>
                <li>Visualize IP addresses</li>
                <ul>
                    <li>Subnet masks</li>
                </ul>
                <li>Colors!</li>
                <li>ASM instruction sets</li>
                <li>Representing numbers</li>
                <ul>
                    <li>Visualize twos-complement</li>
                    <li>Visualize signed/unsigned numbers</li>
                    <li>Floating point</li>
                </ul>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
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