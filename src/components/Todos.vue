<template>
  <div>
    <el-container style="border: 1px solid #eee">
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <Nav />
      </el-aside>
      <el-container>
        <el-header style="text-align: left; font-size: 12px">
          <h2 style="color:white;">代办事项</h2>
        </el-header>
        <el-main>
          <div style="margin-top: 15px;">
            <el-input placeholder="请输入内容" v-model="submitObj.todo" class="input-with-select">
              <el-select v-model="submitObj.todoType" slot="prepend" placeholder="请选择">
                <el-option label="今日代办" value="1"></el-option>
                <el-option label="日后代办" value="2"></el-option>
              </el-select>
              <el-button type="primary" slot="append" icon="el-icon-edit" @click="submit"></el-button>
            </el-input>
            <el-input type="textarea" placeholder="请输入备注" v-model="submitObj.todoNote"> </el-input>
          </div>

          <h3>今日代办</h3>

          <div style="margin-top: 15px;">
            <Todo v-bind:type="1" @updateInfo="updateTodos" ref="todayTodo"/>
          </div>

          <h3>日后代办</h3>

          <div style="margin-top: 15px;">
            <Todo v-bind:type="2" @updateInfo="updateTodos" ref="afterTodo"/>
          </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style>
.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}

.el-select .el-input {
  width: 130px;
}
.input-with-select .el-input-group__prepend {
  background-color: #fff;
}
</style>

<script>
// 代办事项需求说明
// 1.有个输入框新建代办事项
// 2.代办事项有两种类型，一个是今日代办，一个是日后代办
// 3.代办事项有个完成状态
// 4.代办事项可以转换状态
// 5.可以标记删除
//
import axios from 'axios'
import Nav from '@/components/Nav.vue'
import Todo from '@/components/Todo.vue'

// .可以显示常用的快捷键，并进行复制
export default {
  data() {
    return {
      submitObj: {
        todo: 'docker',
        todoType: '1',
        todoNote: '22222'
      },
      todayTodos: [
        {
          todo: 'docker',
          type: '1',
          status: '1'
        }
      ],
      afterTodos: []
    }
  },
  components: {
    Nav,
    Todo
  },
  methods: {
    submit() {
      const path = 'http://127.0.0.1:5000/api/todo/create/'
      axios
        .post(path, this.submitObj)
        .then(response => {
          this.submitObj = {
            todo: '',
            todoType: this.submitObj.todoType,
            todoNote: ''
          }
          console.log('res=>', response)
        })
        .catch(error => {
          console.log(error)
        })
    },
    updateTodos(data) {
      this.$refs.todayTodo.get_data()
      this.$refs.afterTodo.get_data()
    }
  },
  created() {
    // this.getRandom()
  }
}
</script>
