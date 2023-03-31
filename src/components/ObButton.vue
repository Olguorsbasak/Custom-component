<template>
  <button
    :class="`${mode}-button`"
    @mousedown="onMouseDown"
    @mouseup="stopInterval"
    @click="onClick"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 448 512"
      width="20"
      height="20"
    >
      <path
        v-if="mode === 'increment'"
        d="M256 80c0-17.7-14.3-32-32-32s-32 14.3-32 32V224H48c-17.7 0-32 14.3-32 32s14.3 32 32 32H192V432c0 17.7 14.3 32 32 32s32-14.3 32-32V288H400c17.7 0 32-14.3 32-32s-14.3-32-32-32H256V80z"
      />
      <path
        v-else
        d="M432 256c0 17.7-14.3 32-32 32L48 288c-17.7 0-32-14.3-32-32s14.3-32 32-32l352 0c17.7 0 32 14.3 32 32z"
      />
    </svg>
  </button>
</template>

<script>
export default {
  props: {
    mode: {
      type: String,
      default: "increment",
    },
  },
  data() {
    return {
      intervalId: null,
    };
  },
  methods: {
    stopInterval() {
      this.$emit("stop-interval");
    },
    onMouseDown() {
      this.$emit(`start-${this.mode}`);
    },
    onClick() {
      this.$emit(`${this.mode}-click`);
    },
  },
};
</script>

<style lang="scss" scoped></style>
