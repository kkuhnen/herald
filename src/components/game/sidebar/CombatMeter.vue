<template>
  <div class="combat-meters">
    <button class="btn-small btn-reset" @click="onClickReset">
      Reset
    </button>
    <CombatMeterItem
      v-for="(player, player_key) in combat_list"
      class="combat-list-item"
      :key="player_key"
      :player="player"
      :player_key="player_key"
    />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import CombatMeterItem from "./CombatMeterItem.vue";
@Component({
  components: {
    CombatMeterItem,
  }
})
export default class CombatMeter extends Vue {
  get combat_list() {
    return this.$store.state.game.combat_list;
  }

  onClickReset() {
    this.$store.commit("game/combat_meter_reset");
  }
}
</script>

<style lang="scss" scoped>
.btn-reset {
  text-transform: uppercase;
}
</style>
