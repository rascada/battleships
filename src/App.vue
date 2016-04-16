<template>
  <controller></controller>
  <controller></controller>
</template>

<script>
import controller from './components/controller';

export default {
  events: {
    register(name) {
      // this.players.push(name);
      console.log(name, 'register');
    },

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

  methods: {
    getPlayer(name) {
      return this.players[+!this.players.indexOf(name)];
    },

    getOriginPlayer(name) {
      return this.players[this.players.indexOf(name)];
    },
  },

  data() {
    return {
      players: [],
    };
  },

  components: {
    controller,
  },
};
</script>

<style lang='stylus'>
@import '~flexstyl/flex'

body
  height 100vh
  margin 0
  flex styl
  flex center
</style>
