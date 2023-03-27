<template>
  <div class="input-component">
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
        +
      </button>
    </slot>

    <input
      class="input"
      type="text"
      :value="modelValue"
      :placeholder="0"
      min="0"
      @input="onInput"
      style="-moz-appearance: textfield; -webkit-appearance: textfield"
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
        -
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
      this.$emit("update:modelValue", count + 1);
    },
    decrement() {
      let count = 0;
      if (this.modelValue > 0) {
        count = this.modelValue;
      }
      this.$emit("update:modelValue", count - 1);
    },
  },
};
</script>

<style scoped></style>
