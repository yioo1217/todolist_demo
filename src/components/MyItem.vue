<template>
  <li>
    <label>
      <input type="checkbox" :checked = 'todo.done' @change="handleCheck(todo.id)" />
      <span v-show="!todo.isEdit">{{todo.title}}</span>
      <input
      v-show="todo.isEdit"
      :value = 'todo.title'
      type="text"
      @blur="handleBlur(todo,$event)"
      ref="editTitle">
    </label>
    <button class="btn btn-danger" @click="handleDel(todo.id,$event)">删除</button>
    <button v-show="!todo.isEdit" class="btn btn-danger" @click="handleEdit(todo)">编辑</button>
  </li>
</template>

<script>
export default {
  name: 'MyItem',
  props: ['todo'],
  methods: {
    //判断是否完成
    handleCheck(id) {
      // this.checkTodo(id);
      this.$bus.$emit('checkTodo',id)

    },
    handleDel(id) {
      if (confirm('确认删除吗')) {
    //  this.deleteTodo(id)
    this.$bus.$emit('deleteTodo',id)
      }
    },
    //添加input框 if防止有isEdit属性时还继续添加
    handleEdit(todo) {
      //
      if (todo.hasOwnProperty.call('isEdit')) {
        todo.isEdit = true;
      } else {
      this.$set(todo,'isEdit',true)
      }
      this.$nextTick(() => {
        this.$refs.editTitle.focus()
      })
    },
    handleBlur(todo,e) {
      todo.isEdit = false
      console.log(e.target.value);
      if (!e.target.value.trim()) return alert('输入不能为空！');
        this.$bus.$emit('updateTodo',todo.id,e.target.value)

    },
    //失去焦点回调修改逻辑
  }
}
</script>

<style scoped >
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
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
li:hover{
  background-color: #ddd;
}
li:hover button{
display: block;
}
</style>