<template>
  <div class="vitals-view flex flex-grow flex-col">
    <div class="vitals-region">
      <div class="vitals">
        <VitalBar v-for="stat in stats"
          :key="stat.type"
          :type="stat.type"
          :label="stat.label"
          :char="player"
        />
      </div>
    </div>

    <Status />
    <div v-if="is_mobile" class="flex-grow flex flex-col justify-center">
      <Combat @taplook="$emit('taplook')" />
    </div>
    <div v-if="is_mobile" class="mobile-cast-region">
      <Cast
        :message="current_cast"
        v-if="current_cast"
        :key="current_cast.data.expires"
      />
    </div>
    <Skills v-if="$store.state.game.world.allow_combat" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Status from "./Status.vue";
import Skills from "./Skills.vue";
import Combat from "./Combat.vue";
import VitalBar from "./VitalBar.vue";
import Cast from "../consoleviews/Cast.vue";

@Component({
  components: {
    VitalBar,
    Combat,
    Skills,
    Status,
    Cast
  }
})
export default class PanelVitals extends Vue {
  stats: object[] = [];

  constructor() {
    super();
    this.stats.push({type: 'health', label: 'Health'});
    if (this.hasMana) {
      this.stats.push({ type: 'mana', label: 'Mana' });
    }
    this.stats.push({ type: 'stamina', label: 'Stamina' });
  }

  get is_mobile() {
    return this.$store.state.game.is_mobile;
  }

  get player() {
    return this.$store.state.game.player;
  }

  get healthPerc() {
    return (this.player.health / this.player.health_max) * 100 + "%";
  }

  get manaPerc() {
    return (this.player.mana / this.player.mana_max) * 100 + "%";
  }

  get staminaPerc() {
    return (this.player.stamina / this.player.stamina_max) * 100 + "%";
  }

  get hasMana() {
    if (this.player.archetype == "mage" || this.player.archetype == "cleric")
      return true;
    return false;
  }

  get current_cast() {
    return this.$store.state.game.current_cast;
  }
}
</script>

<style lang="scss">
@import "@/styles/colors.scss";
@import "@/styles/fonts.scss";

.vitals-region {
  padding: 0 20px;
  .vitals,
  .effects {
    border-bottom: 1px solid $color-background-light;
    padding-bottom: 12px;
    //padding: 12px 0;
  }

  .vitals {
    display: flex;

    .vital {
      flex-grow: 1;
      width: 100%;

      &:not(:last-child) {
        margin-right: 8px;
      }

      .label {
        @include font-title-light;
        font-size: 15px;
        line-height: 18px;
        color: $color-secondary;
        margin-bottom: 2px;
      }

      .vital-bar {
        background: $color-background-very-light;
        border-radius: 6px;
        height: 6px;
        width: 100%;

        > div {
          width: 0%;
          height: 100%;
          background: $color-secondary;
          border-radius: 6px;
        }
      }
    }
  }
}

.mobile-cast-region {
  margin: 10px 20px;
  height: 20px;

  .cast-message {
    margin: 0 auto;
    width: 33%;

    .progress-bar {
      width: 100%;
    }
  }
}
</style>
