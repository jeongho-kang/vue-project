<template>
    <div class="inputBox shadow">
      <!--input이니까 v-model을 사용한다.-->
      <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo">
      <!-- <button v-on:click="addTodo">add</button> -->
      <span class="addContainer" v-on:click="addTodo">
        <i class="fas fa-plus addBtn"></i>
      </span>

      <modal v-if="showModal" @close="showModal = false">
        <h3 slot="header">경고</h3>
        <span slot="footer" @click="showModal = false">
        할 일을 입력하세요.
          <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
      </modal>
    </div>
  </template>
  
  <script>
  export default {
    data: function(){
      return {
        newTodoItem: "",
      }
    },
    methods: {
      addTodo: function(){
        if (this.newTodoItem !== ''){ // input박스의 입력값이 있을때만 저장한다.
            this.$emit('addTodoItem', this.newTodoItem);
        this.clearInput(); // input박스의 입력값을 초기화
        }
      },
      clearInput: function(){
        this.newTodoItem = ''; // newTodoItem 초기화
      }
    }
  }
  </script>
  
  <style scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    background: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478fb, #8763fb);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
  </style>