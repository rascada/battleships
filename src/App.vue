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
        this.$broadcast('turn', this.players[0]);
      }
    },

    shipDiscovered(id, name) {
      this.$broadcast('shipDiscovered', id, name);
    },

    attack(name, id) {
      const player = this.players[+!this.players.indexOf(name)];

      this.$broadcast('turn', player);
      this.$broadcast('attack', player, id);
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
