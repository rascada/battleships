<template>
  <div v-show='msg || turn' class="controller">
    <panel :turn='turn' :rad='rad' :max-ships='maxShips' type='offensive'></panel>
    <div class='dashboard'> {{ msg }} {{ turn ? 'twój ruch' : '' }} {{ name }} </div>
    <panel type='field' :rad='rad' :max-ships='maxShips'></panel>
  </div>
</template>

<script>
import panel from './panel';

export default {
  props: {
    rad: Number,
    maxShips: Number,
  },

  events: {
    shoot(id) {
      const delay = 1000;

      this.$dispatch('attack', this.name, id);
      this.$broadcast('moveDone', delay);

      setTimeout(() => {
        this.$dispatch('turn', this.name);
      }, delay);
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
      this.name = prompt('podaj imię');
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
      name: '',
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
