<template>
  <div>
    <details class="ui-controls">
      <summary>UI controls</summary>
      <div class="control-group">
        <div>
          <label>
            <input
              type="checkbox"
              v-model="isDark"
              v-on:change="emitIsDarkValue"
            />
            Dark Theme
          </label>
        </div>
        <div>
          <label for="themePicker">Themes: </label>
          <select
            id="themePicker"
            v-model="pickedTheme"
            v-on:change="emitPickedThemeValue"
          >
            <option
              v-for="(theme, index) in themes"
              v-bind:key="index"
              v-bind:value="theme"
            >
              {{ theme }}
            </option>
          </select>
        </div>
      </div>
    </details>
  </div>
</template>

<script>
export default {
  props: {
    themes: Array
  },
  data() {
    return {
      isDark: false,
      pickedTheme: "Avenger"
    };
  },
  methods: {
    emitIsDarkValue() {
      const value = this.isDark;
      this.$emit("onChangeIsDark", value);
    },
    emitPickedThemeValue() {
      const value = this.pickedTheme;
      this.$emit("onChangePickedTheme", value);
    }
  }
};
</script>

<style lang="scss" scoped>
.ui-controls {
  @include ie11() {
    display: none;
  }

  summary {
    margin-bottom: spacer(4);
    display: inline-block;
  }

  .control-group {
    border-top: 1px solid color(tertiary, contrast);
    padding-bottom: spacer(4);
    padding-top: spacer(4);
    display: flex;
    align-items: center;
    justify-content: space-around;
    flex-wrap: wrap;

    > * {
      margin-right: spacer(3);
      margin-left: spacer(3);
      margin-bottom: spacer(3);
    }
  }
}
</style>
