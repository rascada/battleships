<template>
  <div>
    <h1> ustawienia </h1>
    <div>
      <span> średnica pola walki </span>
      <input v-model='rad' number type='number' min='3'>
    </div>
    <div>
      <span> ilość statków (max: {{ maxAmountOfShips }})</span>
      <input v-model='maxShips' number type='number' min='1' :max='maxAmountOfShips'>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    maxShips: Number,
    rad: Number,
  },

  computed: {
    maxAmountOfShips() {
      const rad = this.rad;

      return rad * rad / 1.5 | 0;
    },
  },
};
</script>

<style lang='stylus' scoped>
@import '~flexstyl/flex'

h1
  margin .25em 0
  border-bottom .1em solid

div
  background #09f
  border-radius .2em
  padding 1em
  flex column
  color #fff
  flex line

  input
    input()
</style>
