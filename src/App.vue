<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader @addTodo="addTodo" />
        <TodoList
          :todoList="todoList"
        />
        <TodoFooter
          :todoList="todoList"
          @checkAllTodo="checkAllTodo"
          @clearAllDone="clearAllDone"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";
export default {
  name: "App",
  components: {
    TodoHeader,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todoList: JSON.parse(localStorage.getItem("todoList")) || []
    };
  },
  methods: {
    addTodo(todoObj) {
      this.todoList.unshift(todoObj);
    },
    //修改指定id的选中状态
    changeSelectTodo(id) {
      this.todoList.forEach((todo) => {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 删除指定id的todo
    deleteTodo(id) {
      this.todoList = this.todoList.filter((todo) => {
        return todo.id != id;
      });
    },
    //全选or取消全选
    checkAllTodo(done) {
      this.todoList.forEach((todo) => {
        todo.done = done;
      });
    },
    clearAllDone() {
      this.todoList = this.todoList.filter((todo) => {
        return !todo.done;
      });
    },
    updateTitle(id,title){
      this.todoList.forEach((todo)=>{
        if(todo.id == id){
          todo.title = title;
        }
      })
    }
  },
  watch: {
    todoList: {
      deep:true,
      handler(val) {
        localStorage.setItem("todoList", JSON.stringify(val));
      },
    },
  },
  mounted(){
    this.$bus.$on("changeSelectTodo",this.changeSelectTodo)
    this.$bus.$on("deleteTodo",this.deleteTodo) 
    this.$bus.$on("updateTitle",this.updateTitle) 
  },
  beforeDestroy (){
    this.$bus.$off("changeSelectTodo")
    this.$bus.$off("deleteTodo")
    this.$bus.$off("updateTitle")
  }
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;  
}

.btn-blue {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(103, 172, 200);
  margin-right: 5px;

}


.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
