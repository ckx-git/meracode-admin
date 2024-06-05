<template>
  <div class="view-container">
    <div class="section-container">
      <div class="section-title">考试成绩</div>
      <div class="filter-container">
        <div class="filter-items">
          <el-button type="primary" size="small">发布成绩</el-button>
          <div class="filter-title">成绩筛查</div>
          <el-button type="info" size="small" @click="toResultSummaryPage">成绩总览</el-button>
          <el-button size="small">#1 题</el-button>
          <el-button size="small">#2 题</el-button>
        </div>
        <!-- <div class="filter-items">
          <el-button type="info" size="small">1 - 8</el-button>
          <el-button size="small">9 - 17</el-button>
        </div> -->
      </div>
      <el-table
        :data="tableData"
        border
        style="border: 4px solid #fff;"
        :header-cell-style="{ 'background-color': '#e4e4e4', 'color': 'black', 'border-collapse': 'separate', 'border-spacing': '2px' }"
        :cell-class-name="getCellClass"
        @cell-click="onCellClick"
      >
        <el-table-column prop="name" header-align="center">
          <template slot-scope="scope">
            <div>{{ scope.row.no }}</div>
            <div style="font-weight: bold">{{ scope.row.name }}</div>
          </template>
        </el-table-column>
        <el-table-column prop="ranking" label="单题排名" header-align="center" align="center">
          <template slot-scope="scope">
            <span style="font-weight: bold; font-size: 18px">{{ scope.row.ranking }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="scoreNum" label="得分" header-align="center" align="center">
          <template slot-scope="scope">
            <span style="font-weight: bold; color: #CC0000; font-size: 18px">{{ scope.row.scoreNum }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="memory" label="内存" header-align="center" align="center" />
        <el-table-column prop="time" label="时间" header-align="center" align="center" />
        <el-table-column label="评测结果" header-align="center" align="center">
          <template slot-scope="scope">
            <div>{{ scope.row.runResult }}</div>
          </template>
        </el-table-column>
        <el-table-column prop="time" label="代码" header-align="center" align="center">
          <template slot-scope="scope">
            <div style="cursor: pointer;">
              查看代码
            </div>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
const CELL_BG_RUN_STATUS_MAP = {
  '-1': 'gray',
  '0': 'green',
  '1': 'red',
  '2': 'yellow',
  '3': 'red'
}

export default {
  name: 'ExamResultsView',
  data() {
    return {
      tableData: [
        {
          name: '张浩然',
          no: 'RY93893',
          ranking: '1',
          scoreNum: 100,
          memory: '0.8 MB',
          time: '3s',
          runResult: '运行正确',
          runStatus: 0 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        },
        {
          name: '李闷气',
          no: 'RY93894',
          ranking: '2',
          scoreNum: 100,
          memory: '0.8 MB',
          time: '3s',
          runResult: '运行超时',
          runStatus: 2 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        },
        {
          name: '王思成',
          no: 'RY93895',
          ranking: '3',
          scoreNum: 80,
          memory: '0.8 MB',
          time: '3s',
          runResult: '编译错误',
          runStatus: 3 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        },
        {
          name: '王思成',
          no: 'RY93895',
          ranking: 4,
          scoreNum: 0,
          memory: '0.8 MB',
          time: '3s',
          runResult: '未提交',
          runStatus: -1 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        }
      ]
    }
  },
  methods: {
    getCellClass({ row, column, rowIndex, columnIndex }) {
      console.log(row, column, rowIndex, columnIndex)
      if (columnIndex === 5) {
        return 'cell-bg-' + CELL_BG_RUN_STATUS_MAP[row.runStatus]
      }
      return 'cell-bg-default'
    },
    onCellClick(row, column, cell, event) {
      console.log(arguments)
      this.$router.push({
        name: 'ExamResultsDetail',
        params: {
          id: row.scoreNum
        }
      })
    },
    toResultSummaryPage() {
      this.$router.push('/general/exam-result-summary')
    }
  }
}
</script>

<style lang="scss" scoped>
.view-container {
  padding: 20px;
}
.section-container {
  width: 100%;
  border: 1px solid #eee;
  border-radius: 5px;
  padding: 25px;
  background: #fff;

  .section-title {
    font-size: 18px;
    padding-bottom: 10px;
    margin-bottom: 25px;
    border-bottom: 1px solid #eee;
  }

  .filter-container {
    padding-bottom: 0;

    .filter-items {
      display: flex;
      align-items: baseline;
      margin-bottom: 25px;

      .filter-title {
        margin-left: 50px;
        margin-right: 25px;
      }
    }
  }
}

::v-deep .el-table__row {
  .cell-bg-green {
    background-color: #00ff99;
  }
  .cell-bg-red {
    background-color: #ffcccc;
  }
  .cell-bg-yellow {
    background-color: #ffff99;
  }
  .cell-bg-gray {
    background-color: #bcbcbc;
  }
  .cell-bg-default {
    background-color: #fbfbfb;
  }
}

</style>
