<template>
  <div class="todo">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column label="序号" type="index" width="180"> </el-table-column>
      <el-table-column label="事项" width="180">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>备注: {{ scope.row.todo }}</p>
            <!-- <p>住址: {{ scope.row.address }}</p> -->
            <div slot="reference" class="name-wrapper">
              <el-tag size="medium">{{ scope.row.todo }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button v-if="scope.row.type === '1'" size="mini" type="success" @click="handleEdit(scope.$index, scope.row)">移至日后代办</el-button>
          <el-button v-if="scope.row.type === '2'" size="mini" type="success" @click="handleEdit(scope.$index, scope.row)">移至今日代办</el-button>
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
export default {
  props: ['type'],
  data() {
    return {
      tableData: [
        {
          todo: 'docker',
          type: '1',
          status: '1'
        }
      ]
    }
  },
  methods: {
    handleEdit(index, row) {
      console.log(index, row)
    },
    handleDelete(index, row) {
      console.log(index, row)
    }
  }
}
</script>
