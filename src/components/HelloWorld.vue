<template>
  <div class="block">
    <el-cascader
      v-model="value"
      :options="options"
      @change="handleFilterChange"
      clearable
    ></el-cascader>
  </div>
   <el-table :data="tableDataShown" style="width: 100%" row-key="tag_id">
    <el-table-column size="small" type="selection" :reserve-selection="true"></el-table-column>
    <el-table-column prop="tag_name" label="名称"> </el-table-column> 
    <el-table-column prop="tag_id" label="Tag ID"> </el-table-column>
    <el-table-column prop="tag_type" label="类型"> </el-table-column>
    <el-table-column prop="subscribed_count" label="订阅数"> </el-table-column>
    <el-table-column prop="archive_count" label="投稿数"> </el-table-column>
    <el-table-column prop="featured_count" label="类型"> </el-table-column>
    <el-table-column prop="short_content" label="简述" :show-overflow-tooltip="true"> </el-table-column>
    <el-table-column prop="content" label="详述" :show-overflow-tooltip="true"> </el-table-column>
  </el-table>
  <div class="block">
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      v-model:currentPage="curPage"
      :page-sizes="[5, 15, 30, 60]"
      v-model:page-size="pageSize"
      layout="total, sizes, prev, pager, next"
      :total="totalDataNum"
    ></el-pagination>
  </div>
</template>

<script>
  import {tableData} from '../mock.js'
  export default{
    data() {
      return {
        totalDataNum: tableData.length,
        tableDataShown: [],
        curPage: 1,
        pageSize: 15,
        options: [
          {
            value: "new",
            label: "new_channel"
          },{
            value: "old",
            label: "old_channel"
          }
        ],
        option:'',
        selList:[]
      }
    },
    created() {
      this.tableDataShown = tableData.slice((this.curPage - 1) * this.pageSize, this.curPage * this.pageSize)
    },
    methods: {
      renewTable(){
        this.totalDataNum = tableData.filter(item => item.tag_type.indexOf(this.option) != -1).length
        this.tableDataShown = tableData.filter(item => item.tag_type.indexOf(this.option) != -1).slice((this.curPage - 1) * this.pageSize, this.curPage * this.pageSize)
      },
      handleCurrentChange(val) {
        //console.log(val)
        this.curPage = val
        this.renewTable()
      },
      handleSizeChange(val){  
        this.pageSize = val
        this.renewTable()
      },
      handleFilterChange(val){
        this.option = val != null ? val[0] : ''
        this.renewTable()
      }
    }
  }
  
</script>

<style scoped>

</style>
