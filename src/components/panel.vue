<template>
  <div class="panel" :style='{ width: computedWidth }' v-el:panel>
    {{ remaining }}
    <div v-for='column of rad' class="column">
      <div v-for='row of rad' class="field" @click='shoot(column, row, $event.target)'> {{ ((column) * rad) + row + 1 }} </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ready: false,
      shoots: new Set(),
      ships: new Set(),
    };
  },

  props: {
    rad: Number,
    type: String,
    turn: Boolean,
    maxShips: Number,
  },

  events: {
    shipDiscovered(id) {
      const $field = this.getField(id);

      if (this.type === 'offensive') {
        $field.classList.add('ship');
      }
    },

    attack(target, pos) {
      if (this.type !== 'field') return;

      this.getShoot(pos);
    },

    ready() {
      this.ready = true;
    },
  },

  computed: {
    computedWidth() {
      return `${this.rad * 2}em`;
    },
  },

  methods: {
    getShoot(id) {
      this.attack(id, this.getField(id));
    },

    getField(id) {
      return this.$els.panel
        .children[id[0]]
        .children[id[1]];
    },

    attack(id, $row, dispatch) {
      this.shoots.add(id);
      $row.classList.add('attacked');

      if (dispatch) {
        this.$dispatch('shoot', id);
      } else if (this.ships.has(id)) {
        this.$dispatch('shipPosition', id);
      }
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
      if (!this.ships.has(id) && this.ships.size < this.maxShips) {
        $row.classList.add('ship');

        if (this.ships.add(id).size === this.maxShips) {
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
  width 4em
  text-align center

.field
  box-sizing border-box
  background #09f
  width 2em
  height @width
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
