<script>
import MyComponent from "./components/MyComponent.vue";
export default {
  data() {
    return {
      count: 0,
    };
  },
  watch: {
    count() {
      console.log(this.count);
    },
  },
  components: {
    MyComponent,
  },
};
</script>

<template>
  <div class="page-container">
    <div class="page-content">
      <MyComponent v-model="count" row> </MyComponent>
    </div>
  </div>
</template>

<style scoped>
.page-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
}

.page-content {
  width: 30px;
  height: 30px;
}
</style>
