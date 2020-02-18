<template>
  <div id="app" v-bind:class="settings.bitOrder">
    <label>Value:</label>
    <input type="text"
           v-model="value">
    <span v-show="valueTooHigh">Value too high!</span>
    <button v-on:click="doubleValue"
            v-show="!valueTooHigh">Double value!</button>

    <br>
    <br>

    <Binary v-model="value" v-bind:mode="mode" v-bind:order="settings.bitOrder" v-bind:editable="true" v-bind:raw="false" />
    <br>

    <BitMaskContainer v-bind:any-bit-masks-present="anyBitMasksPresent"
                      v-bind:bit-mask-output="bitMaskOutput"
                      v-bind:bitMasks="bitMasks"
                      v-bind:mode="mode"
                      v-bind:order="settings.bitOrder"
                      v-on:add="addBitmask"
                      v-on:input="updateBitMask(...$event)"
                      v-on:remove="removeBitMask"/>





    <br>
    <Hexadecimal v-model="value" v-bind:mode="mode" v-bind:order="settings.bitOrder" v-show="settings.showHexadecimal" />

    <Settings v-model="settings" />
    <Future />

  </div>
</template>

<script>
import Hexadecimal from "./components/hexadecimal";
import Binary from './components/binary'
import Settings from './components/settings'
import Future from './components/future'
import BitMaskContainer from './components/bit-mask-container'

var availableModes = {
  1: 8,
  2: 16,
  3: 24,
  4: 32
};

export default {
  components: {
    Binary,
    BitMaskContainer,
    Future,
    Hexadecimal,
    Settings
  },
  data: function () {
    return {
      value: 15,
      settings: {
        modeIndex: 1,
        bitOrder: 'big-endian',
        showHexadecimal: true
      },
      bitMasks: [

      ]
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
    },
    anyBitMasksPresent () {
      return this.bitMasks.length;
    },
    bitMaskOutput: function () {
      return this.bitMasks.reduce(function (acc, bitMask) {
        switch (bitMask.operator) {
          case 'and':
            return acc & parseInt(bitMask.value, 2);
          case 'or':
            return acc | parseInt(bitMask.value, 2);
          case 'xor':
            return acc ^ parseInt(bitMask.value, 2);
        }
      }, this.value)
    }
  },
  methods: {
    doubleValue: function () {
      this.value = 2 * this.value;
    },
    addBitmask: function () {
      this.bitMasks.push({
        value: "00000000",
        operator: 'and'
      })
    },
    updateBitMask: function (index, value) {
      this.bitMasks.splice(index, 1, value);
    },
    removeBitMask: function (index) {
      this.bitMasks.splice(index, 1);
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
