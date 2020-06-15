<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- <todo-input v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메서드 이름"></todo-input> -->
    <todo-input v-on:addTodoItem="addOneItem"></todo-input>
    <!-- <todo-list v-bind:내려보낼 프롭스 속성이름="현재 위치의 컴포넌트 데이터 속성"></todo-list> -->
    <todo-list v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></todo-list>
    <todo-footer v-on:removeAll="removeAllItem"></todo-footer>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
export default {
  data(){
    return{
      todoItems:[]
    }
  },
  methods: {
    addOneItem(todoItem){
                let obj = {completed: false, item: todoItem};
                localStorage.setItem(todoItem, JSON.stringify(obj));
                this.todoItems.push(obj);
    },
    removeOneItem(todoItem,index){
      localStorage.removeItem(todoItem.item);
            this.todoItems.splice(index,1);
    },
    toggleOneItem(todoItem, index){
            //todoItem.completed = !todoItem.completed; 아래 코드랑 동일함 바꾼 이유는 이벤트 버스를 이용해서 받은 정보를 굳이 간접적으로 바꿀 필요없이 직접적으로 바꿔야 구성에 좋다
            this.todoItems[index].completed = !this.todoItems[index].completed
            // 로컬 스토리지에 재 저장
            localStorage.removeItem(todoItem.item);
            localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
            console.log(todoItem, index);
    },
    removeAllItem(){
           localStorage.clear();
           this.todoItems=[];
    }
  },
    created() {
      if(localStorage.length > 0){
          for(let i = 0; i < localStorage.length; i++){
             if(localStorage.key(i)!=='loglevel:webpack-dev-server'){
               this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                //this.todoItems.push(localStorage.key(i));
              }
           }
      }
    },
  components:{
    //컴포넌트 태그명 : 컴포넌트 내용
    TodoHeader,
    TodoInput,
    TodoFooter,
    TodoList,
  }
}
</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6;
}
input{
  border-style: groove;
  width: 200px;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>