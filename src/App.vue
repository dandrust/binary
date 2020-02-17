<template>
  <div id="app" v-bind:class="settings.bitOrder">
    Value: <input type="text" v-model="value"> <span v-show="valueTooHigh">Value too high!  Select a higher mode!</span> <button v-on:click="doubleValue">Double value!</button>
    <br>
    <br>
    <Binary v-model="value" v-bind:mode="mode" v-bind:order="settings.bitOrder" />
    <br>
    <Hexadecimal v-model="value" v-bind:mode="mode" v-bind:order="settings.bitOrder" v-show="settings.showHexadecimal" />

    <h1>Settings</h1>
    <h2>Mode</h2>
    <input type="radio" value="1" v-model="settings.modeIndex">
    <label>8 bit</label><br>
    <input type="radio" value="2" v-model="settings.modeIndex">
    <label>16 bit</label><br>
    <input type="radio" value="3" v-model="settings.modeIndex">
    <label>32 bit</label><br>
    <h2>Bit Order</h2>
    <input type="radio" value="big-endian" v-model="settings.bitOrder">
    <label>Big-endian</label><br>
    <input type="radio" value="little-endian" v-model="settings.bitOrder">
    <label>Little-endian</label><br>
    <h2>Hexadecimal</h2>
    <input type="checkbox" v-model="settings.showHexadecimal">
    <label>Show hexadecimal values</label>

    <hr>
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

</template>

<script>
import Hexadecimal from "./components/hexadecimal";
import Binary from './components/binary'

var availableModes = {
  1: 8,
  2: 16,
  3: 32
};

export default {
  components: {
    Hexadecimal,
    Binary,
  },
  data: function () {
    return {
      value: 15,
      settings: {
        modeIndex: 1,
        bitOrder: 'big-endian',
        showHexadecimal: true
      }
    }
  },
  computed: {
    valueTooHigh: function () {
      return this.value > this.upperLimit;
    },
    upperLimit: function () {
      return 2 ** parseInt(this.mode) - 1
    },
    mode: function () {
      return availableModes[this.settings.modeIndex];
    }
  },
  methods: {
    doubleValue: function () {
      this.value = 2 * this.value;
    }
  },
  watch: {
    value: function () {
      if (this.valueTooHigh && availableModes[parseInt(this.settings.modeIndex) + 1]) {
        this.settings.modeIndex ++;
      }
    }
  }
}
</script>

<style>
  .binary-container,
  .byte-container,
  .hexadecimal-container,
  .hexadecimal-group-container {
    display: inline-flex;
    /*flex-flow: row-reverse nowrap;*/
    border: 1px solid lightgray;
    border-radius: 10px;
    justify-content: center;
    margin: 20px;
  }

  .big-endian .binary-container,
  .big-endian .byte-container,
  .big-endian .hexadecimal-container,
  .big-endian .hexadecimal-group-container {
      flex-flow: row-reverse nowrap;
  }

  .little-endian .binary-container,
  .little-endian .byte-container,
  .little-endian .hexadecimal-container,
  .little-endian .hexadecimal-group-container {
      flex-flow: row nowrap;
  }

  .item {
    flex: 1 1 auto;
    background-color: black;
    color: greenyellow;
    font-weight: bold;
    margin: 5px;
    padding: 10px;
    text-align: center;
    border-radius: 10px;
    font-family: monospace;
    font-size: 15px;
  }

  .editing {
    background-color: lightcoral;
    color: white;
  }

  .binary-item {
    width: 25px;
  }
  .hex-item {
    width: 190px;
  }
</style>
