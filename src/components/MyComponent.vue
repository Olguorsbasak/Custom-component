<template>
  <div class="ob-input-component" :class="{ 'input-row': row }">
    <slot
      name="incrementbutton"
      :onmousedown="startIncrement"
      :onmouseup="stopInterval"
      :onclick="stopInterval"
    >
      <button
        class="increment-button"
        @mousedown="startIncrement"
        @mouseup="stopInterval"
        @click="onIncrementClick"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 448 512"
          width="20"
          height="20"
        >
          <path
            d="M256 80c0-17.7-14.3-32-32-32s-32 14.3-32 32V224H48c-17.7 0-32 14.3-32 32s14.3 32 32 32H192V432c0 17.7 14.3 32 32 32s32-14.3 32-32V288H400c17.7 0 32-14.3 32-32s-14.3-32-32-32H256V80z"
          />
        </svg>
      </button>
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
      <button
        class="decrement-button"
        @mousedown="startDecrement"
        @mouseup="stopInterval"
        @click="onDecrementClick"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 448 512"
          width="20"
          height="20"
        >
          <path
            d="M432 256c0 17.7-14.3 32-32 32L48 288c-17.7 0-32-14.3-32-32s14.3-32 32-32l352 0c17.7 0 32 14.3 32 32z"
          />
        </svg>
      </button>
    </slot>
  </div>
</template>

<script>
export default {
  emits: ["increment-click", "update:modelValue", "decrement-click"],
  props: {
    modelValue: {
      type: Number,
    },
    row: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      intervalId: null,
    };
  },
  methods: {
    startIncrement() {
      const count = parseFloat(this.modelValue);
      this.$emit("update:modelValue", count);
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
      const count = parseFloat(this.modelValue);
      this.$emit("update:modelValue", count);
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
    increment() {
      let count = this.modelValue;
      if (count < 100) {
        this.$emit("update:modelValue", count + 1);
      }
    },
    decrement() {
      let count = 0;
      if (this.modelValue > 0) {
        count = this.modelValue;
        if (count > 0) {
          this.$emit("update:modelValue", count - 1);
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.input-row {
  flex-direction: row;
}
</style>
