<template>
  <div class="view-container">
    <div class="section-container">
      <div class="section-title">考试成绩</div>
      <div class="filter-container">
        <div class="filter-items">
          <el-button type="primary" size="small">发布成绩</el-button>
          <div class="filter-title">成绩筛查</div>
          <el-button type="info" size="small">成绩总览</el-button>
          <el-button size="small" @click="toResultSinglePage">#1 题</el-button>
          <el-button size="small" @click="toResultSinglePage">#2 题</el-button>
        </div>
        <div class="filter-items">
          <el-button type="info" size="small">1 - 8</el-button>
          <el-button size="small">9 - 17</el-button>
        </div>
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
        <el-table-column prop="ranking" label="考试排名" header-align="center" align="center">
          <template slot-scope="scope">
            <span style="font-weight: bold; font-size: 18px">{{ scope.row.ranking }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="totalScore" label="总分" header-align="center" align="center">
          <template slot-scope="scope">
            <span style="font-weight: bold; color: #CC0000; font-size: 18px">{{ scope.row.totalScore }}</span>
          </template>
        </el-table-column>
        <el-table-column v-for="(column, index) in columnData" :key="column" :label="column" header-align="center" align="center">
          <template slot-scope="scope">
            <div style="cursor: pointer;">
              <div>{{ scope.row['question_' + (index + 1) + '_score'] }}/{{ scope.row['question_' + (index + 1) + '_runResult'] }}</div>
              <div v-if="scope.row['question_' + (index + 1) + '_runStatus'] == '3'">{{ scope.row['question_' + (index + 1) + '_CompileErrTime'] }}</div>
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
          totalScore: 340,

          question_1_score: 100,
          question_1_runResult: '运行正确',
          question_1_CompileErrTime: '2/15 18:23:34',
          question_1_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_2_score: 90,
          question_2_runResult: '运行正确',
          question_2_CompileErrTime: '2/15 18:23:34',
          question_2_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_3_score: 100,
          question_3_runResult: '运行正确',
          question_3_CompileErrTime: '2/15 18:23:34',
          question_3_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_4_score: 50,
          question_4_runResult: '运行超时',
          question_4_CompileErrTime: '2/15 18:23:34',
          question_4_runStatus: 2, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_5_score: 100,
          question_5_runResult: '运行正确',
          question_5_CompileErrTime: '2/15 18:23:34',
          question_5_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_6_score: 90,
          question_6_runResult: '运行正确',
          question_6_CompileErrTime: '2/15 18:23:34',
          question_6_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_7_score: 100,
          question_7_runResult: '运行正确',
          question_7_CompileErrTime: '2/15 18:23:34',
          question_7_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_8_score: 50,
          question_8_runResult: '运行超时',
          question_8_CompileErrTime: '2/15 18:23:34',
          question_8_runStatus: 2 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        },
        {
          name: '李闷气',
          no: 'RY93894',
          ranking: '2',
          totalScore: 230,

          question_1_score: 100,
          question_1_runResult: '运行正确',
          question_1_CompileErrTime: '2/15 18:23:34',
          question_1_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_2_score: 50,
          question_2_runResult: '运行超时',
          question_2_CompileErrTime: '2/15 18:23:34',
          question_2_runStatus: 2, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_3_score: 80,
          question_3_runResult: '运行正确',
          question_3_CompileErrTime: '2/15 18:23:34',
          question_3_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_4_score: 0,
          question_4_runResult: '编译错误',
          question_4_CompileErrTime: '2/15 18:23:34',
          question_4_runStatus: 3, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_5_score: 100,
          question_5_runResult: '运行正确',
          question_5_CompileErrTime: '2/15 18:23:34',
          question_5_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_6_score: 50,
          question_6_runResult: '运行超时',
          question_6_CompileErrTime: '2/15 18:23:34',
          question_6_runStatus: 2, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_7_score: 80,
          question_7_runResult: '运行正确',
          question_7_CompileErrTime: '2/15 18:23:34',
          question_7_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_8_score: 0,
          question_8_runResult: '编译错误',
          question_8_CompileErrTime: '2/15 18:23:34',
          question_8_runStatus: 3 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误
        },
        {
          name: '王思成',
          no: 'RY93895',
          ranking: '3',
          totalScore: 190,

          question_1_score: 80,
          question_1_runResult: '运行正确',
          question_1_CompileErrTime: '2/15 18:23:34',
          question_1_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_2_score: 50,
          question_2_runResult: '运行超时',
          question_2_CompileErrTime: '2/15 18:23:34',
          question_2_runStatus: 2, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_3_score: 60,
          question_3_runResult: '运行正确',
          question_3_CompileErrTime: '2/15 18:23:34',
          question_3_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_4_score: 0,
          question_4_runResult: '编译错误',
          question_4_CompileErrTime: '2/15 18:23:34',
          question_4_runStatus: 3, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_5_score: 80,
          question_5_runResult: '运行正确',
          question_5_CompileErrTime: '2/15 18:23:34',
          question_5_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_6_score: 50,
          question_6_runResult: '运行超时',
          question_6_CompileErrTime: '2/15 18:23:34',
          question_6_runStatus: 2, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_7_score: 60,
          question_7_runResult: '运行正确',
          question_7_CompileErrTime: '2/15 18:23:34',
          question_7_runStatus: 0, // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

          question_8_score: 0,
          question_8_runResult: '未提交',
          question_8_CompileErrTime: '2/15 18:23:34',
          question_8_runStatus: -1 // -1 未提交 0 正确 1 错误 2 运行超时 3 编译错误

        }
        // {
        //   name: ' MyeyeDo',
        //   no: 'RY93896',
        //   ranking: '4',
        //   totalScore: 120
        // },
        // {
        //   name: '张扬',
        //   no: 'RY93897',
        //   ranking: '5',
        //   totalScore: 110
        // }
      ],
      columnData: [
        '题1', '题2', '题3', '题4', '题5', '题6', '题7', '题8'
      ]
    }
  },
  methods: {
    getCellClass({ row, column, rowIndex, columnIndex }) {
      const _runStatus = row['question_' + (columnIndex - 2) + '_runStatus']

      return 'cell-bg-' + (CELL_BG_RUN_STATUS_MAP[_runStatus] || 'default')
    },
    onCellClick(row, column, cell, event) {
      console.log(arguments)
      this.$router.push({
        name: 'ExamResultsDetail',
        params: {
          id: row.totalScore
        }
      })
    },
    toResultSinglePage() {
      this.$router.push('/general/exam-result-single')
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

// ::v-deep .el-table .el-table__body-wrapper .el-table__body {
//   border-collapse: separate;
//   border-spacing: 3px;
// }

// ::v-deep .el-table::before {
//   height: 0;
// }

// ::v-deep .el-table--enable-row-hover .el-table__body tr:hover > td {
//   background-color: yellow !important;
// }
// .el-table__row:hover > td {
//     background-color: #ffffff !important;

// }

// .el-table__row--striped:hover > td {
//     background-color: #fafafa !important;

// }
</style>
