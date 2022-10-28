<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoList from './components/TodoList.vue';
import TodoInput from './components/TodoInput.vue';
import TodoFooter from './components/TodoFooter.vue';

export default {
  data : function(){
    return{
      todoItems: [],
    }
  },
  methods:{
    addOneItem: function(todoItem){
      //input 컴포넌트에서 이벤트릴 발생시키면 addTodoItem 과 함께 todoItem 도 딸려서 올라온다.
      //이걸 활용해서 객체를 만들고 localStorage 에 넣어준다.
      //그리고 나서 App.vue 의 data() 영역에 있는 todoItems 에도 push 를 하게 되면 바로바로 화면이 갱신이 된다.
      var obj = {completed: false, item: todoItem}
      localStorage.setItem(todoItem,JSON.stringify(obj));//객체를 json을 string 으로 쭉 넣어주는듯
      this.todoItems.push(obj);
   },
   removeOneItem: function(todoItem,index){
     localStorage.removeItem(todoItem.item);
     this.todoItems.splice(index,1);
   },
   toggleComplete: function(todoItem){
        todoItem.completed = !todoItem.completed;
        localStorage.removeItem(todoItem.item);
        localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
      },
    clearAll: function() {
        localStorage.clear();
        this.todoItems = []; // 다시 빈 배열로 만들기
      }
  },
  created:function(){
    //뷰 인스턴스가 생성되자마자 실행되는 lifecycle 훅
    if(localStorage.length > 0) {
        for(var i = 0 ; i<localStorage.length ; i++){
            if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
                // this.todoItems.push(localStorage.key(i));
                
                //완료버튼 토글을 위해 JSON 형식으로 localStorage 에 저장해둔걸 꺼내서 todoItems 에 넣을거다
                //이렇게 하면 	{"completed":false,"item":"aaa"} 이게 나오게 되고 이걸 JSON.parse 하면 다시 객체 형식으로 todoItems에 push 됨
                this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
            }
        }
    }
  },
  components:{
    'TodoHeader' : TodoHeader,
    'TodoInput' : TodoInput,
    'TodoList' : TodoList,
    'TodoFooter' : TodoFooter,
  }
  
}
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  shadow {
    box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
  }
</style>