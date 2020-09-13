<template>
  <div class="combat-meter-item">
    <div class="player-overall-dps" @click="onClickToggleExpanded">
      <span class="icon-toggle" v-if="expanded">-</span>
      <span class="icon-toggle" v-else>+</span>
      <span class="combat-li-label">{{ player.name }}:</span>
      <span class="combat-li-value">{{ Number(totalDPS.dps).toFixed(2) }} DPS</span>
      <span class="player-total-targets color-text-50">({{ getTotalTargets() }})</span>
    </div>
    <div v-if="expanded" class="combat-target-list">
      <div
        v-for="(target, target_key) in player.target_list"
        :key="target_key"
      >
        <div class="combat-target-list-item" v-if="target_key !== 'all'">
          <span class="combat-li-label">{{ target.name }}:</span>
          <span class="combat-li-value">{{ Number(target.dps).toFixed(2) }} DPS</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
@Component
export default class CombatMeterItem extends Vue {
  @Prop() player_key!: string;
  @Prop() player!: PlayerCombatData;
  expanded: boolean = false;

  get totalDPS() {
    return this.player.all;
  }

  getTotalTargets() {
    const count = Object.keys(this.player.target_list).length;
    return (count === 1) ? '1 target' : count + ' targets';
  }

  onClickToggleExpanded() {
    this.expanded = !this.expanded;
  }
}

interface PlayerCombatData {
  name: string;
  target_list: {};
  all: {};
}
</script>

<style lang="scss">
@import "@/styles/colors.scss";

.combat-meter-item {
  border-top: 2px solid $color-background-black;
  margin-top: 2px;
  padding-top: 2px;
}

.combat-target-list {
  background: $color-background-black;
  padding: 0 12px 2px;
}

.player-overall-dps {
  cursor: pointer;
  display: flex;
  flex-flow: row nowrap;
  margin: 0;

  > span {
    flex: 0 0 auto;
    margin: 0 1px;
  }

  > .player-total-targets {
    margin-left: auto;
  }

  .icon-toggle {
    font-family: monospace;
    text-align: center;
    width: 10px;
  }
}

</style>