<template>
  <div id="app">

    <Nav :hue="hue" v-on:newHue="updateHue" />

    <Color v-for="(n, i) in colors" :key="i" :hex="calculateColor(i)" />
    
  </div>
</template>

<script>
  import Nav from './components/Nav'
  import Color from './components/Color';

  import {
    hsv_to_hex
  } from './color';

  export default {
    name: 'app',
    components: {
      Color,
      Nav
    },
    data() {
      return {
        colors: 25,
        hue: 15
      }
    },
    methods: {

      updateHue(n) {
        this.hue = n;
      },
      calculateColor: function (index) {

        var row = Math.floor(index / 5);
        var column = index - row * 5;

        return hsv_to_hex([this.hue, column, 100 - row, 1]).substring(0, 7).toUpperCase();

      }

    }
  }
</script>

<style>
  * {
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
  }

  html,
  body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
  }

  #app {
    position: absolute;
    top: 55px;
    left: 0px;
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    height: calc(100% - 55px);

  }

</style>