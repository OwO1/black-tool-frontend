<template>
  <div>
    <el-container style="height: 500px; border: 1px solid #eee">
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <Nav />
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>查看</el-dropdown-item>
              <el-dropdown-item>新增</el-dropdown-item>
              <el-dropdown-item>删除</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
          <span>王小虎</span>
        </el-header>

        <el-main>
          <el-table :data="tableData" style="width: 100%">
            <el-table-column label="关于" width="180">
              <template slot-scope="scope">
                <i class="el-icon-time"></i>
                <span style="margin-left: 10px">{{ scope.row.about }}</span>
              </template>
            </el-table-column>
            <el-table-column label="命令" width="360">
              <template slot-scope="scope">
                <el-popover trigger="hover" placement="top">
                  <p>介绍: {{ scope.row.introduce }}</p>
                  <div slot="reference" class="name-wrapper">
                    <el-tag size="medium">{{ scope.row.command }}</el-tag>
                  </div>
                </el-popover>
              </template>
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button size="mini" @click="handleCopy(scope.$index, scope.row)">复制</el-button>
                <!-- <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button> -->
              </template>
            </el-table-column>
          </el-table>
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
</style>

<script>
import axios from 'axios'
import Nav from '@/components/nav'

// .可以显示常用的快捷键，并进行复制
export default {
  data() {
    const item = {
      about: 'docker',
      command: 'docker init 123',
      introduce: '初始化'
    }
    return {
      tableData: Array(1).fill(item)
    }
  },
  components: {
    Nav
  },
  methods: {
    go() {
      console.log('1111')
      // this.$router.push('/helloworld/')
    },
    handleCopy(index, row) {
      let command = this.tableData[index].command
      clipboard(command)
      this.getRandomFromBackend()
    },
    getRandom() {
      this.randomNumber = this.getRandomFromBackend()
    },
    getRandomFromBackend() {
      const path = 'http://127.0.0.1:5000/home/'
      axios
        .get(path)
        .then(response => {
          this.tableData = response.data.data
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created() {
    this.getRandom()
  }
}

export function clipboard(text) {
  let inputElement = document.createElement('input')
  inputElement.value = text
  document.body.appendChild(inputElement)
  inputElement.select()
  document.execCommand('copy', true)
  inputElement.parentNode.removeChild(inputElement)
}
</script>
