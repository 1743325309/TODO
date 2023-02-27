<template>
    <div class="todoListBox">
    <el-container>
  <el-header class="header-box">
    <div class="header-left">
    <i class="el-icon-s-order"></i>
        <h2>Todo List</h2>
      </div>
      <input type="text" placeholder="添加ToDo" v-model="todo" @keyup.enter="addTodo">
      <div class="header-right">
        <el-button class="headerAdd" @click="addTodo" type="success" round>添加</el-button>
        <el-button type="danger" @click="clearData" round>清空</el-button>
      </div></el-header>
  <el-main  class="content">
    <div class="content">
    <h2>待办事项
              <span>{{todoLen}}</span>
          </h2>
          <ol>
              <li v-for="(item,index) in todoList" :key="index" v-if="item.done === false">
                  <input type="checkbox" @change="changeTodo(index,true)">
                 <p class="left">{{item.todo}} {{item.time}}</p>
          <el-button  class="content" @click="deleteTodo(index,true)" ></el-button>
              </li>
          </ol>
      <h2>已完成
              <span>{{todoList.length - todoLen}}</span>
          </h2>
          <ol class="done">
              <li v-for="(item,index) in todoList" :key="index" v-if="item.done === true">
                  <input type="checkbox" @change="changeTodo(index,false)" checked='checked'>
                  <p>{{item.todo}} {{item.time}}</p>
                  <el-button  class="content" @click="deleteTodo(index,false)" ></el-button>
              </li>
          </ol>
        </div>
    </el-main>
</el-container>
</div>
</template>

<script>
import dayjs from "dayjs"
import * as Utils from './utils/utils'
export default {
  name: 'App',
  data() {
    return {
      todo:'',
      todoList:[],
      todoLen: 0,
    }
  },
  methods: {
    initTodo () {
        var todoArr = Utils.getItem('todoList')
        if (todoArr) {
        for (let i = 0, len = todoArr.length; i < len; i++) {
            if (todoArr[i].done === false) {
            this.todoLen++
            }
         }
        this.todoList = todoArr
        }
    },
    addTodo(){
          let todoObj = {
              todo:this.todo,
              time:dayjs(new Date).format('YY-MM-DD HH:mm'),
              done:false,
          }
          var tempList = Utils.getItem('todoList')
        if (tempList) {
        tempList.unshift(todoObj)
        Utils.setItem('todoList', tempList)
        } else {
            var tempData = []
            tempData.unshift(todoObj)
            Utils.setItem('todoList', tempData)
        }
          this.todoList.unshift(todoObj);
          this.todoLen++;
          this.todo = '';
      },
      changeTodo(index,done){
          if(done){
              this.todoLen--;
              this.todoList[index].done = true;
              Utils.setItem('todoList', this.todoList)
          }
          else{
              this.todoLen++;
              this.todoList[index].done = false;
              Utils.setItem('todoList', this.todoList)
          }

      },
      deleteTodo(index,done){
          if(done){
              this.todoLen--;
          }
          this.todoList.splice(index,1)
          Utils.setItem('todoList', this.todoList)
      },
      clearData () {
        localStorage.clear()
        this.todoList = []
        this.todoLen = 0
    },

  },
  mounted () {
  this.initTodo()
    }
}
</script>

<style lang="less">
 
button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  color: #fff;
  margin-left: 10px;
}
.header-box input{
    width: 500px;
    margin-right: 30px;
    padding: 10px;
    border: 1px solid #EFEFEF;
}

.todoListBox {
  width: 600px;
  height: 700px;
  background: #FAFAFA;
  border-radius: 10px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  box-sizing: border-box;
  color: #131313;

  .header-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: #ccc 1px solid;

    .header-left {
      display: flex;
      align-items: center;

      i {
        width: 50px;
        height: 30px;
        border-radius: 50%;
        font-size: 30px;
        text-align: center;
        margin-right: 15px;
      }
    }

    .header-right {
      display: flex;
      align-items: center;
      .headerAllSelect {
        background: #C43F38;
      }

      .headerAdd {
        background: #70B870;

      }
    }
  }

  .content {
    
    height: 600px;
    overflow-y: scroll;

.content h2{
    text-align: left;
    position: relative;
}

.content h2 span{
    position: absolute;
    top: 5px;
    right: 5px;
    display: inline-block;
    padding: 0 5px;
    height: 20px;
    border-radius: 20px;
    background-color: #55BB8E;
    font-size: 14px;
    color: #FFF;
    line-height: 22px;
    text-align: center;
}
.content ol{
    list-style: none;
    padding: 0;
}
.content ol li{
    cursor: move;
    height: 32px;
    background-color: #FFF;
    position: relative;
    margin-bottom: 10px;
    padding: 0 45px;
    border-radius: 3px;
    border-left: 5px solid #55BB8E;
}

.content ol li input{
    cursor: pointer;
    width: 22px;
    height: 22px;
    position: absolute;
    top: 2px;
    left: 10px;
}

.content ol li div .left{
    /* display: inline-block; */
    margin: 0;
    padding: 5px;
    text-align: left;




}
.content ol li .content{
  display: inline-block;
    position: absolute;
    top: 2px;
    right: 3px;
    width: 14px;
    height: 12px;
    border-radius: 14px;
    background-color: #CCC;
    line-height: 14px;
    border: 6px double #FFF;
    text-align: center;
    color: #FFF;
    font-size: 14px;
    cursor: pointer;
}
.line-through {
        color: rgba(40, 34, 34, 0.3); // 文字颜色
        text-decoration: line-through rgba(122, 111, 111, 0.8); // 横线颜色
      }

      .content-right {
        display: flex;
        align-items: center;

        button {
          background: #C43F38;
        }
      } 
  }
}
</style>