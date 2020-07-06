<template>
  <div class="todo">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column label="序号" type="index" width="180"> </el-table-column>
      <el-table-column label="事项" width="180">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>备注: {{ scope.row.todo }}</p>
            <div slot="reference" class="name-wrapper">
              <el-tag size="medium">{{ scope.row.todo }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" type="success" @click="handleFinish(scope.$index, scope.row)">完成</el-button>
          <el-button v-if="scope.row.type === 1" size="mini" type="primary" @click="handleTransfer(scope.$index, scope.row)">移至日后代办</el-button>
          <el-button v-if="scope.row.type === 2" size="mini" type="primary" @click="handleTransfer(scope.$index, scope.row)">移至今日代办</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
// type:{
//     '1': 今日代办,
//     '2': 日后代办,
// }
import axios from 'axios'

export default {
  props: ['type'],
  data() {
    return {
      tableData: []
    }
  },
  methods: {
    get_data() {
      const path = 'http://127.0.0.1:5000/api/todo/list/'
      axios
        .get(path, {
          params: {
            todoType: this.type
          }
        })
        .then(response => {
          this.tableData = response.data.data.list
        })
        .catch(error => {
          console.log(error)
        })
    },
    handleEdit(index, row) {
      console.log(index, row)
    },
    handleDelete(index, row) {
      const path = 'http://127.0.0.1:5000/api/todo/delete/'
      let dataId = this.tableData[index]['id']

      axios
        .post(path, { id: dataId })
        .then(response => {
          this.get_data()
        })
        .catch(error => {
          console.log(error)
        })
    },
    handleFinish(index, row) {
      const path = 'http://127.0.0.1:5000/api/todo/update/'
      let currentTodo = this.tableData[index]
      currentTodo.status = '1'
      axios
        .post(path, currentTodo)
        .then(response => {
          this.$emit('updateInfo', '子组件数据')
        })
        .catch(error => {
          console.log(error)
        })
    },
    handleTransfer(index, row) {
      const path = 'http://127.0.0.1:5000/api/todo/update/'
      let currentTodo = this.tableData[index]
      if (this.type === 1) {
        currentTodo.type = '2'
      } else {
        currentTodo.type = '1'
      }
      axios
        .post(path, currentTodo)
        .then(response => {
          this.$emit('updateInfo', '子组件数据')
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created() {
    this.get_data()
  }
}
</script>
