<script>
import MyComponent from "./components/MyComponent.vue";
export default {
  components: {
    MyComponent,
  },
};
</script>

<template>
  <div class="page-container">
    <MyComponent v-model="count"> </MyComponent>
  </div>
</template>

<style scoped></style>
