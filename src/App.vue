<template>
  <div id="app">
    <div id="bg">
      <div class="todo">
        <p>Todo List</p>
        <div class="flex">
          <input id="insert" type="text" name="insert" v-model="newTodo">
          <button class="btn insert" @click="insertTodo">追加</button>
        </div>
        <div class="list" v-for="item in todoLists" :key="item.id">
          <div class="flex">
            <input type="text" v-model="item.todo" >
            <div class="container">
              <button class="btn update" @click="updateTodo(item.id, item.todo)">更新</button>
              <button class="btn delete" @click="deleteTodo(item.id)">削除</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newTodo: "",
      todoLists: [],
    };
  },
  methods: {
    async getTodo() {
      const resData = await axios.get("https://secret-earth-62264.herokuapp.com/api/todo");
      this.todoLists = resData.data.data;
    },
    async insertTodo() {
      const sendData = {
        todo: this.newTodo,
      };
      await axios.post("https://secret-earth-62264.herokuapp.com/api/todo", sendData);
      await this.getTodo();
      this.newTodo = '';
    },
    async updateTodo(id, todo) {
      const sendData = {
        todo: todo,
      };
      await axios.put("https://secret-earth-62264.herokuapp.com/api/todo/" + id, sendData);
      await this.getTodo();
    },
    async deleteTodo(id) {
      await axios.delete("https://secret-earth-62264.herokuapp.com/api/todo/" + id);
      await this.getTodo();
    },
  },
  created() {
    this.getTodo();
  },
};
</script>

<style>
#bg {
  background-color: #191970;
  width: 100vw;
  height: 100vh;
  position: relative;
}
.todo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background-color: #fff;
  border-radius: 20px;
  width: 40%;
}
.todo p {
  font-weight: bold;
  font-size: 24px;
}
input {
  padding: 10px;
  border-radius: 5px;
}
#insert {
  width: 70%;
}
button {
  margin: 2px;
}
.flex{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.btn {
  padding: 10px;
  background-color: white;
  border-radius: 10px;
}
.insert {
  color: pink;
  border: 1px solid pink;
}
.update {
  color: orange;
  border: 1px solid orange;
}
.delete {
  color: greenyellow;
  border: 1px solid greenyellow;
}
</style>