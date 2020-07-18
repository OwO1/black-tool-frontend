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
          <h3>原始字符串</h3>
          <el-input type="textarea" :autosize="{ minRows: 8, maxRows: 15 }" placeholder="请输入内容" v-model="originalString"> </el-input>

          <h3>转换后字符串</h3>

          <el-input type="textarea" :autosize="{ minRows: 8, maxRows: 15 }" placeholder="请输入内容" v-model="transferString"> </el-input>

          <el-button size="mini" type="success" @click="transfer">转换</el-button>
          <el-button size="mini" type="danger">复制</el-button>
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
import axios from 'axios'
import Nav from '@/components/Nav.vue'
import Todo from '@/components/Todo.vue'

export default {
  data() {
    return {
      originalString: '',
      transferString: ''
    }
  },
  components: {
    Nav,
    Todo
  },
  methods: {
    transfer() {
      console.log(this.originalString)
      console.log(this.transferString)
      this.submit()
    },
    submit() {
      const path = 'http://127.0.0.1:5000/api/transfer/info/'
      let submitObj = {
        originalString: this.originalString
      }
      axios
        .post(path, submitObj)
        .then(response => {
          this.transferString = response.data.data
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created() {
    // this.getRandom()
  }
}
</script>
