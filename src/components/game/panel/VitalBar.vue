<template>
  <div :class="'vital ' + type">
    <div v-if="!isCompact" class="label-row">
      <div class="label">{{ label }}</div>
      <div class="amount">{{ vitalVal }}</div>
    </div>
    <div class="vital-bar">
      <div :class="type + '-bar'" :style="{ width: vitalPerc }"></div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class VitalBar extends Vue {
  @Prop() type!: string;
  @Prop() label!: string;
  @Prop() char!: Object;

  get isCompact() {
    return this.label === "";
  }

  get vitalVal() {
    return this.char[this.type];
  }

  get vitalValMax() {
    return this.char[this.type + '_max'];
  }

  get vitalPerc() {
    return (this.vitalVal / this.vitalValMax) * 100 + "%";
  }
}
</script>

<style lang="scss">
@import "@/styles/colors.scss";
@import "@/styles/fonts.scss";

.vital {
  .label-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2px;

    .label {
      @include font-title-light;
      font-size: 15px;
      line-height: 18px;
      color: $color-secondary;
    }

    .amount {
      @include font-text-regular;
      font-size: 11px;
      line-height: 15px;
      //margin-top: 9px;
      margin-bottom: 1px;
      color: $color-text-hex-50;
    }
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
</style>