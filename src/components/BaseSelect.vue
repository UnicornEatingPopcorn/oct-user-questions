<template lang="pug">
.column.is-paddingless
  label.base-select__label {{ label}}
  select.base-input(@change="updateValue")
    option(
      v-for="option in options"
      :key="option.id"
      :value="option.name"
      :selected="option.name === null") {{ option.name }}
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: ""
    },
    value: {
      type: [Number, String]
    },
    options: {
      type: Array,
      required: true
    }
  },
  methods: {
    updateValue(event) {
      this.$emit("updatePropValue", event.target.value);
    }
  }
};
</script>

<style lang="sass">
.base-select__label
  color: white
  font-size: 16px

.base-input
   background-color: #495057 !important
   color: #f7b944 !important
   border-inline-start: 0px !important
   border-block-end: 1px solid black !important
   border-block-start: 0px !important
   border-inline-end: 0px !important
   width: 100%
   font-size: 16px
</style>
