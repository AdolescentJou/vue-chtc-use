<template>
  <input
    type="text"
    :value="todo"
    @blur="onBlur(todo)"
    @input="onInputChange"
    v-bind="$attrs"
  />
  <button v-if="isShowBtn" @click="checkAddTodo(todo)">添加</button>
</template>

<script>
export default {
  props: {
    todo: {
      type: String,
      default: '',
    },
    isShowBtn: {
      type: Boolean,
    },
  },
  methods: {
    checkAddTodo(todo) {
      this.$emit('addTodo', todo);
    },
    onBlur(todo) {
      this.$emit('onblur', todo);
    },
    onInputChange(e) {
      this.$emit('update:todo', e.target.value);
    },
  },
};
</script>

<style></style>
