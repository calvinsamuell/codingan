<template>
  <div class="container" style="margin-top: 20px;">
    <div class="card">
      <div class="class-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" type="text" class="form-control" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
            <button class="btn btn-success" @click="search">Search</button>
          </div>
        </div>
        <list :todos="todos" @deleteTODO="deleteTODO" @doneTodo="doneTodo"/>
        <br/>
        <small>Total TODO : {{ totalTODO }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'
export default{
  components: {List},
  data(){
    return{
      todo: "",
      todos: [],
      activityList: []
      
    }
  },
  mounted(){
    this.todo = JSON.parse(localStorage.getItem('todo'))
  },
  computed:{
    totalTODO(){
      return this.todos.length;
    }
  },
  methods:{
    
    add(){
      if(this.todo != '' && this.todo != null && !this.activityList.includes(this.todo)){
      this.activityList.push(this.todo)
      this.todos.unshift({
        activity: this.todo,
        isDone: false
        });
      this.todo ="";
      this.saveToLocalStorage();
      }
    },
    search(){
      if(this.activityList.includes(this.todo)){
        window.alert('Data ditemukan')
      }else{
        window.alert('Data tidak ditemukan')
      }
    },
    deleteTODO(todoIndex){
      this.todos = this.todos.filter((item,index) =>{
        if(index != todoIndex){
          return item
        }
      });
      this.saveToLocalStorage();
    },
    doneTodo(todoIndex){
      this.todos = this.todos.filter((item,index) => {
        if(index == todoIndex){
          item.isDone = true;
        }

        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage(){
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }

}
</script>

