<template>
  <div>
    <div>
      <el-input
        style="width: 200px; margin-bottom: 5px"
        placeholder="请输入名字"
        @keyup.enter.native="postData"
        prefix-icon="el-icon-search"
        v-model="name"
      >
      </el-input>

      <el-select
        :clearable="clearable"
        @change="sexSearch"
        v-model="sex"
        placeholder="请选择性别"
        style="margin-left: 5px"
      >
        <el-option
          v-for="sex in sexs"
          :key="sex.value"
          :label="sex.label"
          :value="sex.value"
        >
        </el-option>
      </el-select>

      <el-button type="primary" style="margin-left: 5px" @click="postData(name)"
        >搜索</el-button
      >
      <el-button type="success" @click="reset">重置</el-button>
      <el-button type="primary" @click="openDialog">新增</el-button>

      <el-dialog
        title="保存数据"
        :visible.sync="centerDialogVisible"
        width="25%"
        center
        :before-close="beforeClose"
      >
        <el-form ref="form" :model="form">
          <el-form-item
            label="姓名"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="name"
          >
            <el-input
              v-model="form.name"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="年龄"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="age"
          >
            <el-input
              v-model="form.age"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="性别"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="sex"
          >
            <el-radio style="margin-left: 10px" v-model="form.sex" label="1"
              >男</el-radio
            >
            <el-radio v-model="form.sex" label="0">女</el-radio>
          </el-form-item>

          <el-form-item
            label="电话"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="phone"
          >
            <el-input
              v-model="form.phone"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="身份证"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="idCard"
          >
            <el-input
              v-model="form.idCard"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="住址"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="position"
          >
            <el-input
              v-model="form.position"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="备注"
            :label-width="formLabelWidth"
            style="font-weight: bold"
            prop="remark"
          >
            <el-input
              v-model="form.remark"
              autocomplete="off"
              style="width: 300px; margin-left: 8px"
            ></el-input>
          </el-form-item>
        </el-form>

        <span slot="footer" class="dialog-footer">
          <el-button @click="resetForm">取 消</el-button>
          <el-button type="primary" @click="saveOrUpdateUser">确 定</el-button>
        </span>
      </el-dialog>
    </div>

    <el-table
      :data="tableData"
      :header-cell-style="{
        background: '#eef1f6',
        height: '30px',
        color: '#606266',
      }"
    >
      <el-table-column prop="id" label="ID" width="140"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="140"> </el-table-column>
      <el-table-column prop="age" label="年龄" width="200"> </el-table-column>
      <el-table-column prop="sex" label="性别" width="200">
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.sex === 1 ? 'primary' : 'success'"
            disable-transitions
            >{{ scope.row.sex === 1 ? "男" : "女" }}</el-tag
          >
        </template>
      </el-table-column>

      <el-table-column prop="phone" label="电话" width="200"> </el-table-column>

      <el-table-column prop="idCard" label="身份证" width="300">
      </el-table-column>
      <el-table-column prop="position" label="住址" width="300">
      </el-table-column>
      <el-table-column prop="remark" label="备注" width="200">
      </el-table-column>

      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="primary" @click="updateUser(scope.$index, scope.row)"
            >编辑</el-button
          >
          <el-button type="danger" @click="removeUser(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      :background="background"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="pageSizes"
      :page-size="pageSize"
      layout="->,total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>

<script>
const DEFAULT_FORM = {
  id: 0,
  name: "",
  age: "",
  sex: 0,
  phone: "",
  idCard: "",
  position: "",
  remark: "",
};
export default {
  data() {
    return {
      tableData: [],
      currentPage: 1,
      pageSizes: [13, 20, 40, 50],
      pageSize: 13,
      total: 0,
      background: true,
      name: "",
      clearable: true,
      centerDialogVisible: false,
      formLabelWidth: "100px",
      deleteConfirm: false,
      sex: "",
      sexs: [
        {
          value: "1",
          label: "男",
        },
        {
          value: "0",
          label: "女",
        },
      ],
      form: DEFAULT_FORM,
    };
  },
  methods: {
    beforeClose(done) {
      // this.$refs?.form.resetFields();
      //   this.form = JSON.parse(JSON.stringify(this.formDefault));
      this.form = DEFAULT_FORM;
      done();
    },
    showDeleteDialog() {
      this.deleteConfirm = true;
    },
    resetForm() {
      this.$refs.form.resetFields();
      this.centerDialogVisible = false;
    },

    updateUser(index, row) {
      this.centerDialogVisible = true;

      this.$nextTick(() => {
        this.form = {
          id: row.id,
          name: row.name,
          age: row.age,
          sex: row.sex + "",
          phone: row.phone,
          idCard: row.idCard,
          position: row.position,
          remark: row.remark,
        };
      });
    },

    removeUser(index, row) {
      console.log(row);
      this.$confirm("删除该条数据, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        showClose: true, //是否显示右上角关闭按钮
        type: "warning",
      })
        .then(() => {
          this.$axios
            .post("api/removeUsers?id=" + row.id)
            .then((res) => res.data)
            .then((data) => {
              if (data.code === 200) {
                this.$message({
                  showClose: true,
                  message: "操作成功",
                  type: "success",
                });
                this.postData();
              } else {
                this.postData(),
                  this.$message({
                    message: "操作失败",
                    type: "warning",
                  });
              }
            });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },

    doSaveOrUpdate(operate) {
      this.$axios
        .post("api/" + operate, this.form)
        .then((res) => res.data)
        .then((data) => {
          if (data.code === 200) {
            this.centerDialogVisible = false;
            this.postData();
            this.$message({
              showClose: true,
              message: "操作成功",
              type: "success",
            });
          } else {
            this.postData();
            this.$message({
              showClose: true,
              message: "操作失败",
              type: "warning",
            });
          }
        });
    },

    saveOrUpdateUser() {
      if (this.form.id) {
        this.doSaveOrUpdate("updateUsers");
        this.form = DEFAULT_FORM;
      } else {
        this.doSaveOrUpdate("saveUsers");
        this.form = DEFAULT_FORM;
      }
    },
    openDialog() {
      this.centerDialogVisible = true;
    },
    reset() {
      (this.name = ""), (this.sex = "");
      this.postData();
    },
    handleSizeChange(val) {
      this.pageNum = 1;
      this.pageSize = val;
      this.postData();
    },
    handleCurrentChange(val) {
      this.pageNum = val;
      this.postData();
    },

    sexSearch() {
      this.postData();
    },

    postData() {
      this.$axios
        .post("api/getUsers", {
          pageSize: this.pageSize,
          pageNum: this.pageNum,
          name: this.name,
          sex: this.sex || null,
        })
        .then((res) => res.data)
        .then((data) => {
          if (data.code === 200) {
            console.log(data);
            this.total = data.data.total;
            this.tableData = data.data.list;
          } else {
          }
        });
    },
    cellStyle({ row, column, rowIndex, columnIndex }) {
      return "backgroundColor:gray";
    },
  },

  beforeMount() {
    this.postData();
  },
};
</script>

<style>
.totalClass {
  height: calc(100% - 60px);
  overflow: auto;
}
</style>