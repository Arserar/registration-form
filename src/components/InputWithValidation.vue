<template>
  <label>
    <span>{{ title }}*</span>
    <input
      @blur="validation[options.name].$touch()"
      :val="options.val"
      @input="inputHandle($event)"
      :placeholder="options.placeholder ? options.placeholder : ''"
      v-bind:class="{
        error: validation[options.name].$error,
        valid:
          validation[options.name].$dirty && !validation[options.name].$invalid,
      }"
    />
    <div class="errors">
      <p
        v-if="
          validation[options.name].$error &&
          validation[options.name].required.$invalid
        "
      >
        Поле обязательно для заполнения
      </p>
      <p
        v-if="
          validation[options.name].$error &&
          options.condition &&
          validation[options.name][options.condition].$invalid
        "
      >
        {{ options.hint }}
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
    inputHandle(event) {
      this.$emit("input", event.target.value, this.options.name);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
