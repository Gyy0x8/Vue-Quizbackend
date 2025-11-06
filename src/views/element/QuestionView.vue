<template>
  <el-container>
    <!--<el-header>Header</el-header>-->
    <el-header style="font-size: 40px; background-color: rgb(238, 241, 246)"
      >Quiz后台管理</el-header
    >
    <el-container>
      <!---<el-aside width="200px">Aside</el-aside>-->

      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"
              ><i class="el-icon-message"></i
              ><span class="menu-title">系统信息管理</span></template
            >
            <el-menu-item-group>
              <!-- 用户管理 -->
              <el-menu-item index="1-1">
                <router-link to="/user" class="menu-link">用户管理</router-link>
              </el-menu-item>
              <!-- 题目管理 -->
              <el-menu-item index="1-2">
                <router-link to="/question" class="menu-link"
                  >题目管理</router-link
                >
              </el-menu-item>
            </el-menu-item-group>

            <!--
                <el-menu-item-group title="分组2">
                  <el-menu-item index="1-3">选项3</el-menu-item>
                </el-menu-item-group>
                <el-submenu index="1-4">
                  <template slot="title">选项4</template>
                  <el-menu-item index="1-4-1">选项4-1</el-menu-item>
                </el-submenu>
              -->
          </el-submenu>

          <!--
              <el-submenu index="2">
                <template slot="title"><i class="el-icon-menu"></i>导航二</template>
                <el-menu-item-group>
                  <template slot="title">分组一</template>
                  <el-menu-item index="2-1">选项1</el-menu-item>
                  <el-menu-item index="2-2">选项2</el-menu-item>
                </el-menu-item-group>
                <el-menu-item-group title="分组2">
                  <el-menu-item index="2-3">选项3</el-menu-item>
                </el-menu-item-group>
                <el-submenu index="2-4">
                  <template slot="title">选项4</template>
                  <el-menu-item index="2-4-1">选项4-1</el-menu-item>
                </el-submenu>
              </el-submenu>
            -->

          <!--
              <el-submenu index="3">
                <template slot="title"><i class="el-icon-setting"></i>导航三</template>
                <el-menu-item-group>
                  <template slot="title">分组一</template>
                  <el-menu-item index="3-1">选项1</el-menu-item>
                  <el-menu-item index="3-2">选项2</el-menu-item>
                </el-menu-item-group>
                <el-menu-item-group title="分组2">
                  <el-menu-item index="3-3">选项3</el-menu-item>
                </el-menu-item-group>
                <el-submenu index="3-4">
                  <template slot="title">选项4</template>
                  <el-menu-item index="3-4-1">选项4-1</el-menu-item>
                </el-submenu>
              </el-submenu>
            -->
        </el-menu>
      </el-aside>

      <el-main>
        <!-- 顶部的查询表单 -->
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="题目">
            <el-input
              v-model="formInline.user"
              placeholder="请输入题目关键词"
            ></el-input>
          </el-form-item>
          <!--
          <el-form-item label="活动区域">
            <el-select v-model="formInline.region" placeholder="活动区域">
              <el-option label="区域一" value="shanghai"></el-option>
              <el-option label="区域二" value="beijing"></el-option>
            </el-select>
          </el-form-item>
        -->
          <el-form-item>
            <el-button type="primary" @click="onSubmit">查询</el-button>
          </el-form-item>

          <el-form-item>
            <el-button type="success" @click="onAddNewQuestion"
              >添加题目</el-button
            >
          </el-form-item>
        </el-form>

        <!-- 显示的table -->
        <!-- 显示的table -->
        <el-table
          :data="tableData"
          style="width: 100%"
          border
          stripe
          :default-sort="{ prop: 'id', order: 'ascending' }"
          class="question-table"
        >
          <el-table-column type="index" label="序号" width="60" align="center">
          </el-table-column>

          <el-table-column
            prop="question"
            label="题目内容"
            width="300"
            show-overflow-tooltip
          >
          </el-table-column>

          <el-table-column label="选项" width="320">
            <template slot-scope="scope">
              <div class="options-content">
                <div v-if="scope.row.optionsA">A. {{ scope.row.optionsA }}</div>
                <div v-if="scope.row.optionsB">B. {{ scope.row.optionsB }}</div>
                <div v-if="scope.row.optionsC">C. {{ scope.row.optionsC }}</div>
                <div v-if="scope.row.optionsD">D. {{ scope.row.optionsD }}</div>
              </div>
            </template>
          </el-table-column>

          <el-table-column
            prop="answer"
            label="正确答案"
            width="100"
            align="center"
          >
            <template slot-scope="scope">
              <el-tag type="success">{{ scope.row.answer }}</el-tag>
            </template>
          </el-table-column>

          <el-table-column prop="type" label="题型" width="100" align="center">
            <template slot-scope="scope">
              <el-tag
                :type="scope.row.type === '单选题' ? 'primary' : 'warning'"
              >
                {{ scope.row.type }}
              </el-tag>
            </template>
          </el-table-column>

          <el-table-column
            prop="difficulty"
            label="难度"
            width="100"
            align="center"
          >
            <template slot-scope="scope">
              <el-tag
                :type="
                  scope.row.difficulty === '简单'
                    ? 'success'
                    : scope.row.difficulty === '中等'
                    ? 'warning'
                    : 'danger'
                "
              >
                {{ scope.row.difficulty }}
              </el-tag>
            </template>
          </el-table-column>

          <el-table-column label="操作" width="180">
            fixed="right" align="center" >
            <template slot-scope="scope">
              <div class="action-buttons">
                <el-button
                  size="mini"
                  type="primary"
                  @click="handleEdit(scope.$index, scope.row)"
                  icon="el-icon-edit"
                  >编辑</el-button
                >
                <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)"
                  icon="el-icon-delete"
                  >删除</el-button
                >
              </div>
            </template>
          </el-table-column>
        </el-table>

        <br />
        <!-- 分页 -->
        <el-pagination background layout="prev, pager, next" :total="1000">
        </el-pagination>

        <!-- 弹出的添加用户的会话框 -->
        <el-dialog title="添加新题目" :visible.sync="dialogFormVisible">
          <el-form :model="form">
            <el-form-item label="题目" :label-width="formLabelWidth">
              <el-input v-model="form.question" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项A" :label-width="formLabelWidth">
              <el-input v-model="form.optiona" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项B" :label-width="formLabelWidth">
              <el-input v-model="form.optionb" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项C" :label-width="formLabelWidth">
              <el-input v-model="form.optionc" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项D" :label-width="formLabelWidth">
              <el-input v-model="form.optiond" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="答案" :label-width="formLabelWidth">
              <el-input v-model="form.answer" autocomplete="off"></el-input>
            </el-form-item>
            <!---
            <el-form-item label="活动区域" :label-width="formLabelWidth">
              <el-select v-model="form.region" placeholder="请选择活动区域">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
              </el-select>
            </el-form-item>
          -->
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogFormVisible = false"
              >确 定</el-button
            >
          </div>
        </el-dialog>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      dialogFormVisible: false,
      form: {
        question: "",
        optiona: "",
        optionb: "",
        optionc: "",
        optiond: "",
        answer: "",
      },
      formInline: {
        user: "",
        region: "",
      },
      tableData: [
        {
          id: "1",
          question: "中国的首都是哪个城市？",
          optionsA: "北京",
          optionsB: "上海",
          optionsC: "广州",
          optionsD: "深圳",
          answer: "A",
          type: "单选题",
          difficulty: "简单",
        },
        {
          id: "2",
          question: "以下哪个不是编程语言？",
          optionsA: "Java",
          optionsB: "Python",
          optionsC: "HTML",
          optionsD: "C++",
          answer: "C",
          type: "单选题",
          difficulty: "简单",
        },
        {
          id: "3",
          question: "Vue.js的主要特点是什么？",
          optionsA: "响应式",
          optionsB: "组件化",
          optionsC: "虚拟DOM",
          optionsD: "以上都是",
          answer: "D",
          type: "单选题",
          difficulty: "中等",
        },
        {
          id: "4",
          question: "以下哪些是JavaScript的数据类型？",
          optionsA: "字符串",
          optionsB: "数字",
          optionsC: "布尔值",
          optionsD: "以上都是",
          answer: "D",
          type: "多选题",
          difficulty: "简单",
        },
      ],
    };
  },
  methods: {
    onSubmit() {
      console.log("submit!");
    },
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
    },
    onAddNewQuestion() {
      this.dialogFormVisible = true;
    },
  },
};
</script>

<style>
.menu-title {
  font-size: 16px;
}

.menu-link {
  text-decoration: none;
  display: block;
  width: 100%;
  height: 100%;
  font-size: 15px;
  text-align: center;
}

/* 表格样式优化 */
.question-table {
  margin-top: 20px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.options-content div {
  margin-bottom: 4px;
  font-size: 13px;
}

.options-content div:last-child {
  margin-bottom: 0;
}

/* 表头样式 */
.el-table .el-table__header-wrapper th {
  background-color: #f5f7fa;
  font-weight: bold;
  color: #606266;
}

/* 操作按钮样式 */
.action-buttons {
  display: flex;
  justify-content: center;
  gap: 8px; /* 按钮间距 */
}

/* 或者如果使用按钮组 */
.el-button-group .el-button {
  margin: 0;
}
</style>