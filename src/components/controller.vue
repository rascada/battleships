<template>
  <div v-show='msg || turn' class="controller">
    <panel :turn='turn' type='offensive'></panel>
    <div class='dashboard'> {{ msg }} {{ turn ? 'twój ruch' : '' }} </div>
    <panel type='field'></panel>
  </div>
</template>

<script>
import panel from './panel';

export default {
  events: {
    shoot(id) {
      this.$dispatch('attack', this.name, id);
      setTimeout(() => {
        this.$dispatch('turn', this.name);
      }, 1000);
    },

    turn(name, turn) {
      if (name === this.name) {
        this.turn = turn;
      }
    },

    shipDiscovered(id, name) {
      return this.name !== name;
    },

    shipPosition(id) {
      this.$dispatch('shipDiscovered', id, this.name);
    },

    settedShips() {
      this.$dispatch('ready', this.name);
      this.$broadcast('ready');
      this.msg = '';
    },

    attack(target) {
      return target === this.name;
    },
  },

  ready() {
    this.$dispatch('register', this.name);
  },

  data() {
    return {
      name: Math.random() * 1000,
      msg: 'Ustaw swoje statki',
      turn: false,
    };
  },

  components: {
    panel,
  },
};
</script>

<style scoped lang='stylus'>
@import '~flexstyl/flex'

.controller
  flex line
  flex column
  flex center
  margin auto

.dashboard
  background #333
  color #fff
  padding .5em
  height 1em
  width 100%
</style>
