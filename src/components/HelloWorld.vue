<template>
  <div>

    <el-table :data="tableData" style="wpbiIdth: 100%">
      <el-table-column label="序号" wpbiIdth="180">
        <template slot-scope="scope">
          <p>{{ scope.row.order }}</p>
        </template>
      </el-table-column>
      <el-table-column label="类型" wpbiIdth="180">
        <template slot-scope="scope">
          <div v-if="!activeEditIndex.includes(scope.$index)">
            <span class="select-tags" closable v-for="(type, index) of scope.row.typeData" :key="index">{{ type
            }}</span>
          </div>
          <div v-if="activeEditIndex.includes(scope.$index)">
            <el-tree-select popoverClass="test-class-wrap" v-model="scope.row.typeData" :styles="styles"
              :selectParams="selectParams" :treeParams="scope.row.treeParams" ref="treeSelect"></el-tree-select>
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
  </div>

</template>

<script>
import ElTreeSelect from './TreeSelect.vue';
export default {
  data() {
    return {
      activeEditIndex: [],
      tableData: [{
        order: '1',
        typeData: ['一级 1', '二级 1-1'],
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
        treeParams: {}
      }, {
        order: '2',
        typeData: ['二级 1-1'],
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
        treeParams: {}
      }],
      styles: {
        width: '300px'
      },
      // 单选value为字符串，多选为数组
      values: this.isSingle ? '' : [],

      // 选择器参数
      selectParams: {
        clearable: true,
        placeholder: '请输入内容'
      },
      treeParams: {
        clickParent: true,
        // 是否显示搜索框
        filterable: true,
        // 支持直接传入 element tree 的 props
        // 点击父节点和子节点是否关联
        'check-strictly': true,
        // 是否默认展开所有节点
        'default-expand-all': true,
        // 是否在点击节点的时候选中节点
        'expand-on-click-node': false,
        props: {
          "children": "children",
          "label": "cn",
          "disabled": "disabled",
          "value": "cn"
        },
      }
    }
  },

  components: {
    ElTreeSelect
  },

  created() {
    this.tableData.forEach(item => {
      item.treeParams = this.treeParams;
      item.treeParams.data = item.treeData;
    })
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

.use-select-tree {
  width: 300px;
  margin: 100px auto;
}

.el-select-dropdown__item {
  padding: 0;
  height: auto;
}
</style>