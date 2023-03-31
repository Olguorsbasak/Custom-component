<template>
  <div class="ob-input-component" :class="{ 'input-row': row }">
    <slot
      name="incrementbutton"
      :onmousedown="startIncrement"
      :onmouseup="stopInterval"
      :onclick="stopInterval"
    >
      <ObButton
        :mode="'increment'"
        :model-value="modelValue"
        @increment-click="onIncrementClick"
        @start-increment="startIncrement"
        @stop-interval="stopInterval"
      ></ObButton>
    </slot>
    <input
      class="input"
      type="number"
      :value="modelValue"
      :placeholder="0"
      @input="onInput"
    />
    <slot
      name="decrementbutton"
      :onmousedown="startDecrement"
      :onmouseup="stopInterval"
      :onclick="stopInterval"
    >
      <ObButton
        :mode="'decrement'"
        :model-value="modelValue"
        @decrement-click="onDecrementClick"
        @start-decrement="startDecrement"
        @stop-interval="stopInterval"
      ></ObButton>
    </slot>
  </div>
</template>

<script>
import ObButton from "./ObButton.vue";

export default {
  components: {
    ObButton,
  },
  emits: ["increment-click", "update:modelValue", "decrement-click"],
  props: {
    modelValue: {
      type: Number,
    },
    row: {
      type: Boolean,
      default: false,
    },
    minValue: {
      type: Number,
      default: 0,
    },
    maxValue: {
      type: Number,
      default: 100,
    },
  },
  data() {
    return {
      intervalId: null,
    };
  },
  methods: {
    startIncrement() {
      this.increment();
      this.intervalId = setTimeout(() => {
        this.intervalId = setInterval(() => {
          this.increment();
        }, 50);
      }, 500);
    },
    onInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },
    onIncrementClick() {
      this.stopInterval();
      this.$emit("increment-click", this.modelValue);
    },
    onDecrementClick() {
      this.stopInterval();
      this.$emit("decrement-click", this.modelValue);
    },

    startDecrement() {
      this.decrement();
      this.intervalId = setTimeout(() => {
        this.intervalId = setInterval(() => {
          this.decrement();
        }, 50);
      }, 500);
    },
    stopInterval() {
      clearInterval(this.intervalId);
    },
    increment(step = 1, max = 100) {
      let count = this.modelValue + step;
      if (count > max) {
        count = max;
      }
      this.$emit("update:modelValue", count);
    },

    decrement(step = 1, min = 0) {
      let count = this.modelValue - step;
      if (count < min) {
        count = min;
      }
      this.$emit("update:modelValue", count);
    },
  },
};
</script>

<style lang="scss" scoped>
.input-row {
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>
