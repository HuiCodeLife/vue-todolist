<template>
  <div class="todo-footer" v-show="todoList.length">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneCount }}</span> / 全部{{ todoList.length }}
    </span>
    <button
      class="btn btn-danger"
      @click="handlerClearAllDone()"
      v-show="doneCount"
    >
      清除已完成任务
    </button>
  </div>
</template>

<script>
export default {
  name: "TodoFooter",
  props: ["todoList"],
  data() {
    return {};
  },
  methods: {
    handlerClearAllDone() {
      if (confirm("确定要删除已完成的吗")) {
        // this.clearAllDone();
        this.$emit("clearAllDone");
      }
    },
  },
  computed: {
    //已完成个数
    doneCount() {
      return this.todoList.reduce((pre, cur) => pre + (cur.done ? 1 : 0), 0);
    },
    isAll: {
      get() {
        return this.doneCount == this.todoList.length;
      },
      set(val) {
        // console.log(val);
        // this.checkAllTodo(val);
        this.$emit("checkAllTodo", val);
      },
    },
  },
};
</script>

<style soped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
