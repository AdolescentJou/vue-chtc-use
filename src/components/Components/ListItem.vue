<template>
  <template v-if="!isEdit"
    ><span @dblclick="checkoutState(index)" :class="{ text: item.complete }">{{
      editTittle
    }}</span>
    <button @click="completeTodo()" v-if="!item.complete">完成</button>
    <button @click="deleteTodo(index)" v-else>删除</button>
  </template>
  <template v-else>
    <InputBox
      @keyup.enter="editTodo()"
      @onblur="editTodo()"
      v-model:todo="editTittle"
      :isShowBtn="false"
    />
  </template>
</template>

<script>
import { reactive, toRefs } from 'vue';
import InputBox from './InputBox.vue';
export default {
  props: {
    item: {
      type: Object,
    },
    index: {
      type: Number,
    },
  },
  components: {
    InputBox,
  },
  emits: ['completeTodo', 'editTodo', 'deleteTodo'],
  setup(props, { emit }) {
    const state = reactive({
      //这里不能结构使用，不然会失去响应
      editTodo: props.item,
      editTittle: props.item.tittle,
      isEdit: false,
    });
    // 实现完成函数
    function completeTodo() {
      state.editTodo.complete = true;
      emit('editTodo', state.editTodo, props.index);
    }

    function editTodo() {
      state.isEdit = false;
      state.editTodo.tittle = state.editTittle;
      emit('editTodo', state.editTodo, props.index);
    }
    function deleteTodo() {
      emit('deleteTodo', props.index);
    }
    //双击切换编辑状态
    function checkoutState() {
      state.isEdit = true;
    }
    return {
      ...toRefs(state),
      completeTodo,
      checkoutState,
      deleteTodo,
      editTodo,
    };
  },
};
</script>

<style>
.text {
  text-decoration: line-through;
}
</style>
