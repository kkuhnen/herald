<template>
  <div class="inventory indented">
    <template v-if="message.data.inventory.length">
      <div>You can sell:</div>
      <ol class="list mb-4">
        <li class="inventory-item" v-for="(item) in message.data.inventory" :key="item.key">
          <span
            v-if="isLastMessage"
            class="interactable"
            :class="[item.quality]"
            v-interactive="{target: item}"
            :key="item.key + '-interactable'"
          >{{ item.name }}</span>
          <span v-else :class="[item.quality]" :key="item.key">{{ item.name }}</span>
          for {{item.cost}} gold
        </li>
      </ol>
    </template>
    <template v-else>
      <div>You have nothing to sell.</div>
    </template>

    <div class="gold-inv color-secondary">You have {{ message.data.gold }} gold.</div>
  </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
@Component
export default class GameOfferInventory extends Vue {
  @Prop() message!: any;

  get isLastMessage() {
    return (
      this.$store.state.game.last_message[this.message.type] == this.message
    );
  }
}
</script>

<style lang="scss" scoped>
@import "@/styles/colors.scss";
</style>