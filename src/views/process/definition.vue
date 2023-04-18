<template>
  <div class="app-container">
    <div class="filter-container">
      <el-form :model="queryParams" ref="queryForm" :inline="true" v-show="showSearch" label-width="68px">
        <el-form-item label="名称" prop="name">
          <el-input
            v-model="queryParams.name"
            placeholder="请输入名称"
            clearable
            size="small"
            @keyup.enter.native="handleQuery"
          />
        </el-form-item>
        <el-form-item label="开始时间" prop="deployTime">
          <el-date-picker clearable size="small"
                          v-model="queryParams.deployTime"
                          type="date"
                          value-format="yyyy-MM-dd"
                          placeholder="选择时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
          <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
        </el-form-item>
      </el-form>
    </div>

    <el-table v-loading="loading" fit :data="list" border   @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55" align="center" />
      <el-table-column label="流程编号" align="center" prop="deploymentId" :show-overflow-tooltip="true"/>
      <el-table-column label="流程标识" align="center" prop="flowKey" :show-overflow-tooltip="true" />
      <el-table-column label="流程分类" align="center" prop="category" />
      <el-table-column label="流程名称" align="center" width="120" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          <el-button type="text" @click="handleReadImage(scope.row.deploymentId)">
            <span>{{ scope.row.name }}</span>
          </el-button>
        </template>
      </el-table-column>
      <el-table-column label="业务表单" align="center" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          <el-button v-if="scope.row.formId" type="text" @click="handleForm(scope.row.formId)">
            <span>{{ scope.row.formName }}</span>
          </el-button>
          <label v-else>暂无表单</label>
        </template>
      </el-table-column>
      <el-table-column label="流程版本" align="center">
        <template slot-scope="scope">
          <el-tag size="medium" >v{{ scope.row.version }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="状态" align="center">
        <template slot-scope="scope">
          <el-tag type="success" v-if="scope.row.suspensionState === 1">激活</el-tag>
          <el-tag type="warning" v-if="scope.row.suspensionState === 2">挂起</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="部署时间" align="center" prop="deploymentTime" width="180"/>
      <el-table-column label="操作" width="250" fixed="right"class-name="small-padding fixed-width">
        <template slot-scope="scope">
<!--          <el-button @click="handleLoadXml(scope.row)" icon="el-icon-edit-outline" type="text" size="small">设计</el-button>-->
          <el-button @click="handleAddForm(scope.row)" icon="el-icon-edit-el-icon-s-promotion" type="text" size="small" v-if="scope.row.formId == null">配置主表单</el-button>
          <el-button @click="handleUpdateSuspensionState(scope.row)" icon="el-icon-video-pause" type="text" size="small" v-if="scope.row.suspensionState === 1">挂起</el-button>
          <el-button @click="handleUpdateSuspensionState(scope.row)" icon="el-icon-video-play" type="text" size="small" v-if="scope.row.suspensionState === 2">激活</el-button>
          <el-button @click="handleDelete(scope.row)" icon="el-icon-delete" type="text" size="small" v-hasPermi="['system:deployment:remove']">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="queryParams.page" :limit.sync="queryParams.limit"  />


  </div>
</template>

<script>
import Pagination from '@/components/Pagination' // secondary package based on el-pagination


export default {
name: "definition",
  data() {
    return {
      queryParams: {},
      showSearch: false,
      list: [],
      total: 0,
      loading: false
    }
  },
  components: { Pagination },
  methods: {
    handleQuery() {

    },
    resetQuery() {

    },
    handleSelectionChange() {

    }
  }
}
</script>

<style scoped>

</style>
