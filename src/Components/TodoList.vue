<template>
    <div>
      <transition-group name="list" tag="ui">
        <!--뷰 데이터의 개수만큼 화면에 표시한다.-->
        <li v-for="(todoItem, index) in propsdata" v-bind:key="todoItem.item" class="shadow">
          <i class="checkBtn fas fa-check" v-bind:class="{checkBtnCompleted: todoItem.completed}" 
          v-on:click="toggleComplete(todoItem, index)"></i>
          <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
          <i class="fas fa-trash-alt"></i>
        </span>
        </li>
      </transition-group>
    </div>
  </template>
  
  <script>
  // TodoList에 컴포넌트 props 속성 추가
  export default {
    props:['propsdata'],
    data: function(){
      return {
        todoItems: []
      }
    },
    methods: {
      removeTodo: function(todoItem, index){
        this.$emit('removeItem',todoItem,index);
      },
      toggleComplete: function(todoItem){
        this.$emit('toggleEvent', todoItem,index);
      }
    },
    created: function(){
      // 뷰 인스턴스가 생성되자마자 실행되는 lifecyle 훅
      if (localStorage.length > 0) {
        for(var i = 0; i< localStorage.length; i++){
          if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
            this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
          }
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }
  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  .checkBtnCompleted {
    color: #b3adad;
  }
  .textCompleted {
    text-decoration: line-through;
    color: #b3adad;
  }
  .removeBtn {
    margin-left: auto;
    color: #de4343;
  }
  </style>