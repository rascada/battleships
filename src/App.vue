<template>
  <controller
    :rad='rad'
    :max-ships='maxShips'>
  </controller>

  <settings
    v-show='players.length === 0'
    :max-ships.sync='maxShips'
    :rad.sync='rad'>
  </settings>

  <controller
    :rad='rad'
    :max-ships='maxShips'>
  </controller>
</template>

<script>
import controller from './components/controller';
import settings from './components/settings';

export default {
  components: {
    controller,
    settings,
  },

  data() {
    return {
      rad: 3,
      maxShips: 3,
      players: [],
    };
  },

  methods: {
    getPlayer(name) {
      return this.players[+!this.players.indexOf(name)];
    },

    getOriginPlayer(name) {
      return this.players[this.players.indexOf(name)];
    },
  },

  events: {
    ready(name) {
      this.players.push(name);
      if (this.players.length === 2) {
        this.$broadcast('turn', this.players[0], true);
      }
    },

    shipDiscovered(id, name) {
      this.$broadcast('shipDiscovered', id, name);
    },

    turn(name) {
      this.$broadcast('turn', name, false);

      setTimeout(() => {
        alert('Zmiana gracza');
        this.$broadcast('turn', this.getPlayer(name), true);
      }, 10);
    },

    attack(name, id) {
      this.$broadcast('attack', this.getPlayer(name), id);
    },
  },
};
</script>

<style lang='stylus'>
@import '~simple-look/register'
@import '~flexstyl/flex'

*
  font-family 'Roboto Condensed', sans-serif

body
  height 100vh
  margin 0
  flex styl
  flex center
</style>
