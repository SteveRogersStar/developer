<template>
  <div>
    <h4 class="title">筛选条件</h4>
    <hr/>
    <div class="filters">
      <el-form :inline="true" :model="filter" label-width="6em">
        <el-form-item label="序号">
          <el-input v-model="filter.id" placeholder="请输入序号" ></el-input>
        </el-form-item>
        <el-form-item label="项目名称">
          <el-input v-model="filter.name" placeholder="输入项目名称" ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="search" icon="el-icon-search">搜索</el-button>
          <el-button type="info" @click="clear" icon="el-icon-delete">清空</el-button>
        </el-form-item>
      </el-form>
    </div>
    <h4 class="title">前端项目列表</h4>
    <hr/>
    <v-datagrid :columns="columns" :data-url="dataUrl" :count-url="countUrl" :params="params" :toolbar="toolbar"/>
  </div>
</template>

<script>
import { endCountApi, endListApi } from '@/config/api/inserv-api'
import { formatDate, removeBlank } from '@/config/utils'

export default {
  name: 'end',
  data () {
    return {
      filter: {
        id: null,
        name: null
      },
      dataUrl: endListApi,
      countUrl: endCountApi,
      params: {},
      toolbar: [{
        title: '新增',
        icon: 'el-icon-plus',
        handler: this.edit
      }],
      columns: [
        {field: 'id', header: '序号', sort: 'id', width: 100},
        {field: 'name', header: '项目名称', sort: 'name', width: 300},
        {field: 'prefix', header: '前缀', sort: 'prefix', width: 100},
        {field: 'header', header: 'Header', sort: 'header', width: 100},
        {field: 'mark', header: 'mark', sort: 'mark', width: 320},
        {field: 'createTime', header: '添加时间', sort: 'create_time', width: 200, formatter: (row, index, value) => formatDate(value)},
        {
          field: 'action',
          header: '操作',
          width: 130,
          actions: [
            {
              text: '编辑',
              handler: (row) => {
                this.$router.push(`/project/list/edit/${row.id}`)
              }
            }
          ]
        }
      ]
    }
  },
  mounted () {
    if (this.action) {
      this.columns.push({field: 'action', header: '操作', sort: 'id', width: 230, actions: this.action || []})
    }
  },
  methods: {
    search () {
      this.params = removeBlank(this.filter)
    },
    clear () {
      this.params = {}
      this.filter = { name: '' }
    },
    edit (r) {
      this.$router.push(`/project/list/edit`)
    }
  },
  components: {
    'v-datagrid': () => import('@/components/datagrid')
  }
}
</script>

<style lang="scss" scoped>
  @import 'index.scss';
</style>
