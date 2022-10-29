<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
    <MyHeader @addTodo = 'addTodo'></MyHeader>
    <MyList :todo ='todo' :checkTodo="checkTodo" :deleteTodo="deleteTodo"></MyList>
    <MyFooter :todo ='todo' @checkAllTodo="checkAllTodo"  @clearAllTodo="clearAllTodo"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>

import MyFooter from "./components/MyFooter.vue";
import MyHeader from "./components/MyHeader.vue";
import MyList from "./components/MyList.vue";
//父传子用props app传一个函数给header header调用时携带数据 先传给list 再传给item 爷传父 父传子
export default {
  name: 'App',
  components: {
    MyFooter,
    MyHeader,
    MyList
  },
  data() {
    return {
      todo: JSON.parse(localStorage.getItem('todo')) || []
      //将本地存储的字符串转成对象存入todo数组 第一次使用读取时是null 读取length会报错 || 或上一个空数组解决
    }
  },
  //数据在哪 方法函数写在哪
  methods: {
    addTodo(todoObj) {
      this.todo.unshift(todoObj);

    },
    checkTodo(id) {
      //遍历选择 过滤出需要的todo对象
      this.todo.forEach((todo) => {
        if (todo.id === id) {
          //布尔值取反
          todo.done = !todo.done;
        }
      });
    }, updateTodo(id,title) {
      //遍历选择 过滤出需要的todo对象
      this.todo.forEach((todo) => {
        if (todo.id === id) {
          //布尔值取反
          todo.title = title;
        }
      });
    },
    deleteTodo(id) {
      //这个过滤器filter()内的是形参可以随便写但不能用todo 和 关键字do
      this.todo = this.todo.filter(tod => tod.id !== id)
    },
    checkAllTodo(done) {
      this.todo.forEach((tod) => {
        tod.done = done
      })
    },
    clearAllTodo() {
      this.todo = this.todo.filter((tod) => {
        return !tod.done
      })
    }
  },
  watch: {

    todo: {
      deep: true,
      handler(value) {
        localStorage.setItem('todo', JSON.stringify(value))
        //将对象转成字符串
      }
    }
  },
  mounted() {
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
    this.$bus.$on('deleteTodo',this.deleteTodo)
  },
  beforeDestroy() {
    this.$bus.$off(['checkTodo','deleteTodo','updateTodo'])
  },
}
</script>
<!-- 总体的样式不用加scoped -->
<style>
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
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
