<template>
  <h2>{{ name }}Todo List demo</h2>
  <InputBox
    @keyup.enter="addTodo()"
    @onblur="addTodo"
    @addTodo="addTodo"
    v-model:todo="todo"
    placeholder="请输入todo"
    :isShowBtn="true"
  />
  <div>完成数量: {{ completeTotal }}</div>
  <ul>
    <li v-for="(item, index) in showList" :key="index">
      <ListItem
        :item="item"
        @deleteTodo="deleteTodo"
        @editTodo="editTodo"
        :index="item.id"
      />
    </li>
  </ul>
  <ButtonGroup v-model:showState="showState" />
</template>

<script>
import { reactive, computed, toRefs } from 'vue';
import InputBox from './Components/InputBox.vue';
import ListItem from './Components/ListItem.vue';
import ButtonGroup from './Components/ButtonGroup';
export default {
  props: ['name'],
  components: {
    InputBox,
    ListItem,
    ButtonGroup,
  },
  //接收props和context
  setup() {
    //响应式方法,定义响应式对象
    const state = reactive({
      todo: '',
      todoList: [],
      showList: computed(() => {
        return filterList(state.showState, state.todoList);
      }),
      showState: 'all',
      completeTotal: computed(() => {
        return state.todoList.filter((todo) => todo.complete).length;
      }),
    });
    function filterList(showState, todoList) {
      switch (showState) {
        case 'all':
          return todoList;
        case 'complete':
          return todoList.filter((item) => item.complete);
        case 'uncomplete':
          return todoList.filter((item) => !item.complete);
      }
    }
    // 实现添加函数
    function addTodo() {
      if (state.todo == '') return;
      let id = 0;
      if (state.todoList.length) {
        id = state.todoList[state.todoList.length - 1].id + 1;
      }
      state.todoList.push({
        id: id,
        tittle: state.todo,
        complete: false,
      });
      state.todo = '';
    }

    //实现修改函数
    function editTodo(todo, index) {
      state.todoList = state.todoList.map((item) => {
        if (item.id == index) {
          return todo;
        } else {
          return item;
        }
      });
    }
    //实现删除函数
    function deleteTodo(index) {
      state.todoList = state.todoList.filter((item) => item.id != index);
    }
    
    //定义一个watcher监听字符变化
    //     watch(state.todo, (value, preValue) => {
    //       console.log(`从${preValue}变成了${value}`);
    //     });

    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      editTodo,
      filterList,
    };
  },
};
</script>

<style>
.text {
  text-decoration: line-through;
}
</style>
