<template>
  <div class='effect' :class="{ friendly: isFriendly, hostile: isHostile}">
    <div v-if="$store.state.game.player_config.combat_brief" class='brief flex'>

      <div class='effect-code flex-grow'>
        [ {{ effectLabel }} ]
        <span v-if="message.data.target === player_key">Effect Start</span>
        <span v-else>{{ capfirst(message.data.target_data.keyword) }} Effect Start</span>
      </div>
      <div class='duration nowrap mr-2'>{{ message.data.duration}} sec</div>
    </div>
    <div class='effect' v-else>
        {{ message.text }}
    </div>
  </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import { capfirst } from "@/core/utils.ts";

@Component
export default class EffectMessage extends Vue {
  @Prop() message!: any;
  @Prop() previousMessage!: any;
  @Prop() index!: number;

  player_id: number;
  player_key: string;

  capfirst = capfirst;

  constructor() {
    super();
    this.player_id = this.$store.state.game.player_id;
    this.player_key = `player.${this.player_id}`;
  }

  get isFriendly() {
    return (
      this.message.data.friendly && this.message.data.target === this.player_key
    );
  }

  get isHostile() {
    return (
      !this.message.data.friendly &&
      this.message.data.target === this.player_key
    );
  }

  get effectLabel() {
    return this.$store.state.game.world.labels.effects[this.message.data.code];
  }
}
</script>

<style lang="scss" scoped>
@import "@/styles/colors.scss";
@import "@/styles/fonts.scss";
.effect {
  &.friendly {
    color: $color-green;
    @include font-text-regular;
    .brief {
      color: $color-green !important;
    }
  }

  &.hostile {
    color: $color-red;
    @include font-text-regular;
    .brief {
      color: $color-red !important;
    }
  }
}
</style>