<template>
  <div class="hello">
    <h1>ToDoリスト</h1>
     <li class="status" v-for="item in items" :key="item.id">
      <input type="radio" name="show" :id="item.id" :value="item.value" v-model="current">
      <label :for="item.id">{{item.label}}</label>
     </li>


    <table class="todo-list">
     <thead>
      <tr>
        <td>ID</td>
        <td>コメント</td>
        <td>状態</td>
        <td></td>
      </tr>
     </thead>
     <thead>
       <tr v-for="(item,index) in computedTodos" :key="index">
         <td>{{index + 1}}</td>
         <td>{{item.task}}</td>
         <td><button @click="change(item)">{{ labels[item.state] }}</button></td>
         <!-- <td><input type="button" value="作業中" v-model="tasks.done"></td> -->
         <td><button @click="del(index)">削除</button></td>
       </tr>
     </thead>
    </table>

   <h1>新規タスクの追加</h1>
   <div v-on:submit.prevent>
     <label for="input-todo-box"></label>
     <input type="text" v-model="value">
     <button @click="add">追加</button>
   </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      items: [
      {id: 'all', value: -1, label: 'すべて'},
      {id: 'doing', value: 0, label: '作業中'},
      {id: 'done', value: 1, label: '完了'}
    ],
    tasks: [],
    task: '',
    value: '',
    show: 0,
    done: 0,
    current: -1,
    }
  },
  methods: {
    add() {
      this.tasks.push({
        task: this.value,
        state: 0,
      });
      this.value = '';
    },
    del(index) {
      this.tasks.splice(index, 1);
    },
    change: function(item) {
      item.state = !item.state ? 1:0
    },
  },
   computed: {
     labels() {
      return this.items.reduce(function (a, b) {
        return Object.assign(a, { [b.value]: b.label })
      }, {})
    },
    computedTodos: function () {
      return this.tasks.filter(function (el) {
        return this.current < 0 ? true : this.current === el.state
      }, this)
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .status {
    display: inline;
  }
</style>
