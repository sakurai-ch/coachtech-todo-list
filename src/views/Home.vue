<template>
  <div class="home">
    <div class="outer-box">
      <h1>Todo List</h1>
      <div class="add">
        <input type="text" class="box input-box" v-model="input">
        <button class="button add-button" @click="add">追加</button>
      </div>
      <div class="update-delete" v-for="item in list" :key="item.id">
        <input type="text" class="box list-box" v-model="item.todo">
        <div class="update-delete-button">
          <button class="button update-button" @click="update(item)">更新</button>
          <button class="button delete-button" @click="del(item)">削除</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'Home',
  components: {
  },
  data(){
    return{
      input: "",
      list: [],
    };
  },
  methods: {
    async get(){
      const todo_list = await axios.get("http://127.0.0.1:8000/api");
      this.list = [];
      todo_list.data.data.forEach(element => {
        this.list.push(
          {id: element.id, todo: element.todo}
        );
      });
    },

    async add(){
      await axios.post("http://127.0.0.1:8000/api", {todo: this.input});
      this.input = "";
      this.get();
    },
    async update(item){
      await axios.put("http://127.0.0.1:8000/api", {id: item.id, todo: item.todo});
      this.get();
    },
    async del(item){
      await axios.delete("http://127.0.0.1:8000/api", {data:{id: item.id}});
      this.get();
    },
  },
  created(){
    this.get();
  }
}
</script>

<style scoped>
.home{
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.outer-box{
  width: 50%;
  border-radius: 10px;
  background-color: white;
  padding: 5px 30px 25px 30px;
  margin: 50px auto;
}

h1{
  font-size: 24px;
  margin-bottom: 10px;
}

.add{
  margin-bottom: 15px;
  display: flex;
  justify-content: space-between;
}

.box{
  height: 34px;
  border: solid 2px #E5E5E5;
  border-radius: 5px;
  margin: 0;
  padding: 1px;
  padding-left: 5px;
  font-size: 14px;
  text-align: 14px;
}

.input-box{
  width: 80%;
  margin-right: auto;
  margin-left: 0;
}

.button{
  height: 40px;
  width: 60px;
  border: solid 2px;
  border-radius: 5px;
  margin: 0;
  margin-left: 5px;
  padding: 1px;
  font-size: 12px;
  text-align: 14px;
  font-weight: bold;
  background-color: white;
  cursor: pointer;
}

.add-button{
  color: #DC70FB;
  border-color: #DC70FB;
}

.add-button:hover{
  background-color: #DC70FB;
  color: white;
  transition: 0.5s ;
}

.list-box{
  width: 30%;
}

.update-delete{
  margin-bottom: 5px;
  display: flex;
  justify-content: space-between;
}

.update-delete-button{
  display: flex;
  justify-content: space-between;
}

.update-button{
  color: #EC8784;
  border-color: #EC8784;
}

.update-button:hover{
  background-color: #EC8784;
  color: white;
  transition: 0.5s ;
}

.delete-button{
  color: #7CFADC;
  border-color: #7CFADC;
}

.delete-button:hover{
  background-color: #7CFADC;
  color: white;
  transition: 0.5s ;
}

@media screen and (max-width : 480px){
  .box{
    height: 67px;
    width: 60%;
  }

  .add-button{
    height:72px;
    width: 55px;
  }

  .update-delete-button{
    width: 60px;
    display: initial;
  }

  .update-button{
    height:36px;
    width: 55px;
    margin-bottom: 2px;
  }

  .delete-button{
    height:36px;
    width: 55px;
  }
}
</style>
