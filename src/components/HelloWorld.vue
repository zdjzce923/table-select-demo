<template>
  <el-table :data="tableData" style="wpbiIdth: 100%">
    <el-table-column label="序号" wpbiIdth="180">
      <template slot-scope="scope">
        <p>{{ scope.row.order }}</p>
      </template>
    </el-table-column>
    <el-table-column label="类型" wpbiIdth="180">
      <template slot-scope="scope">
        <div v-if="!activeEditIndex.includes(scope.$index)">
          <span class="select-tags" closable v-for="(type, index) of scope.row.typeData" :key="index">{{ type }}</span>
        </div>
        <div v-if="activeEditIndex.includes(scope.$index)">
          <treeselect 
            :multiple="true" 
            :options="scope.row.treeData" 
            :default-expand-level="1" 
            :flat="true" 
            zIndex="9999"
            placeholder="请输入" 
            v-model="scope.row.typeData" 
            :append-to-body="true" />
        </div>
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
          v-if="!activeEditIndex.includes(scope.$index)">编辑</el-button>
        <el-button size="mini" @click="handleSave(scope.$index, scope.row)"
          v-if="activeEditIndex.includes(scope.$index)">保存</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import Treeselect from '@riophae/vue-treeselect'
// import the styles
import '@riophae/vue-treeselect/dist/vue-treeselect.css'
export default {
  data() {
    return {
      activeEditIndex: [],
      tableData: [{
        order: '1',
        typeData: ['一级 1', '二级 1-1'],
      }],
      treeData: [{
        pbiId: 1,
        cn: '一级 1',
        children: [{
          pbiId: 2,
          cn: '二级 1-1',
          children: [{
            pbiId: 3,
            cn: '三级 1-1-1'
          }, {
            pbiId: 4,
            cn: '三级 1-1-2'
          }]
        }]
      }, {
        pbiId: 5,
        cn: '一级 2',
        children: [{
          pbiId: 6,
          cn: '二级 2-1'
        }, {
          pbiId: 7,
          cn: '二级 2-2'
        }]
      }],
    }
  },

  components: {
    Treeselect
  },

  created() {
    this.deepSelectDataHandle()
  },

  methods: {
    handleEdit(index, row) {
      console.log('row:', row)
      this.activeEditIndex.push(index);
    },
    handleSave(index, row) {
      console.log('row:', row)
      this.activeEditIndex = this.activeEditIndex.filter(item => item !== index);
    },
    deepSelectData(treeData) {
      if (!treeData || !treeData.length) return [];
      let data = [];
      treeData.forEach(item => {
        data.push({
          id: item.cn,
          label: item.cn,
          children: item.children ? this.deepSelectData(item.children) : []
        })
      })
      return data;
    },
    deepSelectDataHandle() {
      this.tableData.forEach(item => {
        item.treeData = this.deepSelectData(this.treeData);
        console.log('item.treeData:', item.treeData)
      })
    },
  }
}
</script>
<style>
.select-tags {
  padding: 10px;
  background: #E6F2FC;
  color: #5FA7E4;
  border-radius: 15px;
  margin-right: 10px;
}
</style>