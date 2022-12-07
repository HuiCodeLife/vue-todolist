<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleChange(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        :value="todo.title"
        ref="inputTitle"
        @blur="handleBlur(todo, $event)"
        v-show="todo.isEdit"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      class="btn btn-blue"
      v-show="!todo.isEdit"
      @click="handleEdit(todo)"
    >
      修改
    </button>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo"],
  methods: {
    // 修改状态
    handleChange(id) {
      this.$bus.$emit("changeSelectTodo", id);
      // this.changeSelectTodo(id);
    },
    // 点击删除按钮
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        // this.deleteTodo(id);
        this.$bus.$emit("deleteTodo", id);
      }
    },
    // 点击修改按钮
    handleEdit(todo) {
      //仅第一次添加isEdit属性
      if (todo.hasOwnProperty("isEdit")) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }
      // 下一次渲染后执行输入框聚焦 ，解决聚焦失败
      this.$nextTick(function () {
        this.$refs.inputTitle.focus();
      });
    },
    // 失去输入框焦点时修改数据
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) {
        return alert("不允许内容空");
      }
      this.$bus.$emit("updateTitle", todo.id, e.target.value);
    },
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li:hover {
  background-color: #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover button {
  display: block;
}
</style>
