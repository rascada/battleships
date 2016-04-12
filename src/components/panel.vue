<template>
  <div class="panel" v-el:panel>
    {{ remaining }}
    <div v-for='column of rad' class="column">
      <div v-for='row of rad' class="field" @click='shoot(column, row, $event.target)'> {{ column }} - {{ row }} </div>
    </div>
  </div>
</template>

<script>
const maxShips = 2;

export default {
  data() {
    return {
      rad: 4,
      ready: false,
      shoots: new Set(),
      ships: new Set(),
    };
  },

  props: {
    type: String,
    turn: Boolean,
  },

  events: {
    attack(target, pos) {
      if (this.type !== 'field') return;

      this.getShoot(pos);
    },

    ready() {
      this.ready = true;
    },
  },

  methods: {
    getShoot(id) {
      const $row = this.$els.panel.children[id[0]].children[id[1]];

      this.attack(id, $row);
    },

    attack(id, $row, dispatch) {
      this.shoots.add(id);
      $row.classList.add('attacked');

      if (dispatch) this.$dispatch('shoot', id);
    },

    shoot(x, y, $row) {
      const id = `${x}${y}`;

      if (this.type !== 'offensive') {
        this.setShip(id, $row);
        return;
      } else if (!this.ready || !this.turn) return;

      if (!this.shoots.has(id)) {
        this.attack(id, $row, true);
      } else alert('znowu?');
    },

    setShip(id, $row) {
      if (!this.ships.has(id) && this.ships.size < maxShips) {
        $row.classList.add('ship');

        if (this.ships.add(id).size === maxShips) {
          this.$dispatch('settedShips');
        }
      }
    },
  },
};
</script>

<style scoped lang="stylus">
@import '~flexstyl/flex';

.panel
  width 18em
  text-align center

.field
  box-sizing border-box
  background #09f
  width 4em
  height 4em
  color #fff
  cursor pointer
  border .1em solid #fff

  flex line
  flex center

.ship
  background #888

.attacked
  background #09f - 50%

  &.ship
    background indianred
</style>
