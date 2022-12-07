<template>
  <div class="todo-header">
    <h1 class="title">个人任务表</h1>
    <input
      type="text"
      v-model="title"
      @keyup.enter="handleAdd"
      placeholder="请输入你的任务名称，按回车键确认"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "TodoHeader",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    handleAdd() {
      // 判断输入的不为空字符串
      if (this.title.trim() === "") return;
      // 待添加todo事件
      const todoObj = { id: nanoid(), title: this.title, done: false };
      // this.addTodo(todoObj);
      this.$emit("addTodo",todoObj)
      // 清空输入框
      this.title = ''
    }
  },
};
</script>

<style scoped>
/*header*/
.title {
  text-align: center;
}
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
