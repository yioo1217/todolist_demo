<template>
  <div class="todo-footer" v-show="total">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{doneTotal}} </span> / 全部{{total}}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>


export default {
  name: 'MyFooter',
  props: ['todo','checkAllTodo','clearAllTodo'],
  computed: {
    total() {
      return this.todo.length
  },
  doneTotal() {
    // forEach写法
    //   let i = 0;
    //   this.todo.forEach(tod => {
    //     if (tod.done) i++;
    //   });
    // return i
      //reduce写法
       const x = this.todo.reduce((pre,current)=>{
					return pre + (current.done ? 1 : 0)
       }, 0)
    return x;
    },
    isAll: {
      get(){
        return this.total === this.doneTotal && this.total >0
      },
      set(value) {
        this.$emit('checkAllTodo',value);
      }
    }
  },
  methods: {
//     checkAll(value) {
// this.checkAllTodo(value)
//     }
    clearAll() {
      this.$emit('clearAllTodo');
}
  }
}
</script>

<style>

</style>