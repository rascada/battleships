<template>
  <div v-show='msg || turn' class="controller">
    <panel v-show='ready' :turn='turn' :rad='rad' :max-ships='maxShips' type='offensive'></panel>
    <div class='dashboard'> {{ msg }} {{ name }} </div>
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

  components: {
    panel,
  },

  ready() {
    this.$dispatch('register', this.name);
  },

  data() {
    return {
      name: '',
      msg: 'Ustaw swoje statki',
      ready: false,
      turn: false,
    };
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

    ready() {
      this.ready = true;
      return true;
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
};
</script>

<style scoped lang='stylus'>
@import '~flexstyl/flex'

.controller
  flex line
  flex column
  flex center
  margin auto
  padding .25em
  background #09f

.dashboard
  background #333
  color #fff
  padding .5em
  height 1em
  width 100%
</style>
