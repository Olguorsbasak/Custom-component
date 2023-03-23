<template>
  <div class="my-component">
    <button
      class="increment-button"
      @mousedown="startIncrement"
      @mouseup="stopInterval"
      @mouseleave="stopInterval"
    >
      +
    </button>
    <input
      class="input"
      type="text"
      :value="count"
      :placeholder="0"
      min="0"
      @input="count = parseFloat($event.target.value) || 0"
      style="-moz-appearance: textfield; -webkit-appearance: textfield"
    />
    <button
      class="decrement-button"
      @mousedown="startDecrement"
      @mouseup="stopInterval"
      @mouseleave="stopInterval"
    >
      -
    </button>
    <p class="count">Count: {{ count }}</p>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      count: this.value || 0,
      intervalId: null,
    };
  },
  methods: {
    startIncrement() {
      this.count = parseFloat(this.count);
      this.increment();
      this.intervalId = setTimeout(() => {
        this.intervalId = setInterval(() => {
          this.increment();
        }, 50);
      }, 500);
    },
    startDecrement() {
      this.count = parseFloat(this.count);
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
      this.count++;
    },
    decrement() {
      if (this.count > 0) {
        this.count--;
      } else {
        this.count = 0;
      }
    },
  },
};
</script>

<style scoped></style>
<link rel="stylesheet/scss" href="~/assets/styles/MyComponent.scss" />
