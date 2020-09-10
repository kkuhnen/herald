<template>
  <div>
    <form>
      <div class="form-group">
        <input class="clickcast-cmd" v-model="command" />
      </div>
    </form>
    <div
      class="char-list-item"
      v-for="char in sortedChars"
      :key="char.key"
      v-interactive="{ target: char, side: 'right' }"
    >
      <div class="char-name-target">
        <span class="char-nameplate">
          <span v-if="isHostile(char)">*</span>
          <span :class="{ hostile: isHostile(char), interactable: isFriendly(char) }" @click="onClickCast(command, char)">
            <span class="char-name">{{ char.name }}</span>
            <span class="char-hp-perc" v-if="isFriendly(char)">({{ percentHP(char) }}%)</span>
          </span>
          <span v-if="isHostile(char)">*</span>
        </span>
        <span class="char-target color-text-50" v-if="char.target">
          > {{ char.target.keywords.split(' ')[0] }}
        </span>
      </div>
      <VitalBar v-if="isFriendly(char)"
        :char="char"
        type="health"
        label=""
      />
      <!-- <VitalBar v-if="isFriendly(char)"
        :char="char"
        type="mana"
        label=""
      /> -->
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import VitalBar from "../panel/VitalBar.vue";

@Component({
  components: {
    VitalBar,
  }
})
export default class Chars extends Vue {
  player: any;
  command: string = "";

  constructor() {
    super();
    this.player = this.$store.state.game.player;
  }

  get chars() {
    return this.$store.state.game.room_chars;
    return this.$store.state.game.room.chars;
  }

  get sortedChars() {
    const sortedChars = [...this.chars];
    return sortedChars.sort(function(a,b) {
      let keyA = a.key.toUpperCase();
      let keyB = b.key.toUpperCase();
      if (keyA > keyB) {
        return -1;
      }
      if (keyA < keyB) {
        return 1;
      }
      return 0;
    });
  }

  isHostile(char) {
    if (
      this.player.core_faction != char.core_faction &&
      char.char_type === "player"
    )
      return true;
    return false;
  }

  isFriendly(char) {
    if (
      this.player.core_faction == char.core_faction &&
      char.char_type === "player"
    )
      return true;
    return false;
  }

  percentHP(char) {
    return Math.round((char.health / char.health_max) * 100);
  }

  onClickCast(command, char) {
    if (command === "") {
      return;
    }
    if (this.player.key === char.key) {
      this.$store.dispatch("game/cmd", `${command}`);
    }
    else if (this.isFriendly(char)) {
      this.$store.dispatch("game/cmd", `${command} ${char.name}`);
    }
  }
}
</script>

<style lang="scss">
@import "@/styles/colors.scss";

.clickcast-cmd {
  &:not(:focus) {
    background: inherit;
    color: inherit;
  }
}

.char-name-target {
  display: flex;
  flex-flow: row nowrap;
}

.char-nameplate {
  flex: 0 0 auto;
}

.char-target {
  flex: 1 1 auto;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-left: 12px;
}

.char-list-item {
  margin-bottom: 6px;

  .hostile {
    color: $color-red;
  }

  .vital {
    margin-top: 3px;
  }

  .vital {
    .vital-bar {
      > .health-bar {
        background: $color-red;
      }
      > .mana-bar {
        background: $color-blue;
      }
    }
  }
}

.char-list-item:last-child {
  margin-bottom: 0;
}

</style>
