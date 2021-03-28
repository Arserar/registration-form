<template>
  <label>
    <span>{{ title }}</span>
    <input v-model="value" />
  </label>
</template>

<script>
export default {
  name: "RegularInput",
  data() {
    return {
      value: "",
    };
  },
  props: {
    title: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
