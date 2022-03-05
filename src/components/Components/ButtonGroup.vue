<template>
  <label>过滤条件</label>
  all<input
    type="radio"
    name="radio"
    value="all"
    :checked="showState == 'all'"
    @change="$emit('update:showState', 'all')"
  />
  complete<input
    type="radio"
    name="radio"
    value="complete"
    :checked="showState == 'complete'"
    @change="$emit('update:showState', 'complete')"
  />
  uncomplete<input
    type="radio"
    name="radio"
    value="uncomplete"
    :checked="showState == 'uncomplete'"
    @change="$emit('update:showState', 'uncomplete')"
  />
</template>

<script>
export default {
  props: {
    showState: {
      type: String,
    },
  },
  methods: {
    handleChange: function (e) {
      this.$emit('showState', e.target.defaultValue);
    },
  },
};
</script>

<style></style>
