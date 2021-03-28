<template>
  <label>
    <span>{{ title }}*</span>
    <select
      @blur="validation[options.name].$touch()"
      @input="selectHandle($event)"
      :value="options.val"
      :multiple="options.multiple"
      v-bind:class="{
        error: validation[options.name].$error,
        valid:
          validation[options.name].$dirty && !validation[options.name].$invalid,
      }"
    >
      <option selected disabled value>{{ options.defaultValue }}</option>
      <option v-for="opt in options.opts" :key="opt" :value="opt">
        {{ opt }}
      </option>
    </select>
    <div class="errors">
      <p
        v-if="
          validation[options.name].$error &&
          validation[options.name].required.$invalid
        "
      >
        Поле обязательно для заполнения
      </p>
    </div>
  </label>
</template>

<script>
export default {
  name: "InputWithValidation",
  props: {
    validation: Object,
    options: Object,
    title: String,
  },
  methods: {
    selectHandle(event) {
      this.$emit("select", event.target.value, this.options.name);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
