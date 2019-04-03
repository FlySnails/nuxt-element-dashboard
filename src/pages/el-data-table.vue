<template>
  <el-data-table v-bind="tableConfig" :hasOperation="false" ref="elDataTable">
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="small"
          @click="handleOperate(scope.row, 'onDefaultView')"
          type="primary"
        >
          查看
        </el-button>
        <el-button
          size="small"
          @click="handleOperate(scope.row, 'onDefaultEdit')"
        >
          编辑
        </el-button>
        <el-button size="small" @click="handleOperate(scope.row, 'onOroff')">
          {{ scope.row.status === 'on' ? '下架' : '上架' }}
        </el-button>
      </template>
    </el-table-column>
  </el-data-table>
</template>

<script>
import moment from 'moment'
const LANGUAGE_OPTIONS = ['java', 'javaScript', 'php']
const STATUS_OPTIONS = [
  {value: 'on', label: '上架'},
  {value: 'off', label: '下架'}
]
export default {
  data() {
    return {
      tableConfig: {
        url: '/api/users',
        columns: [
          {type: 'selection'},
          {
            prop: 'name',
            label: '组件名称'
          },
          {
            prop: 'type',
            label: '分类'
          },
          {
            prop: 'version',
            label: '版本'
          },
          {
            prop: 'dev_language',
            label: '开发语言'
          },
          {
            prop: 'update_time',
            label: '更新时间',
            formatter: row => moment(row.update_time).format('YYYY-MM-DD')
          },
          {
            prop: 'status',
            label: '状态',
            formatter: row => (
              <span class={row.status}>
                {row.status === 'on' ? '上架' : '下架'}
              </span>
            )
          }
        ],
        searchForm: [
          {
            $type: 'input',
            $id: 'name',
            label: '组件名称',
            $el: {
              placeholder: '请输入'
            }
          },
          {
            $type: 'select',
            $id: 'type',
            label: '分类',
            $el: {
              placeholder: '请选择'
            },
            $options: LANGUAGE_OPTIONS.map(v => ({label: v, value: v}))
          },
          {
            $type: 'select',
            $id: 'status',
            label: '状态',
            $el: {
              placeholder: '请选择'
            },
            $options: STATUS_OPTIONS
          }
        ],
        form: [
          {
            $type: 'select',
            $id: 'dev_language',
            label: '开发语言',
            rules: [
              {required: true, message: '请选择开发语言', trigger: 'blur'}
            ],
            $options: LANGUAGE_OPTIONS.map(v => ({label: v, value: v})),
            $el: {
              placeholder: '请选择'
            }
          },
          {
            $type: 'input',
            $id: 'name',
            label: '名称',
            rules: [
              {
                required: true,
                message: '请输入名称',
                trigger: 'blur',
                transform: v => v && v.trim()
              }
            ],
            $el: {placeholder: '请输入'}
          }
        ]
      }
    }
  },
  methods: {
    /**
     * 操作
     * @param row  行数据
     * @param type 操作类型
     */
    handleOperate(row, type) {
      if (type === 'onOroff') {
        this.$refs.elDataTable.getList()
        return
      }
      this.$refs.elDataTable[type](row)
    }
  }
}
</script>
<style lang="less" scoped>
.el-data-table {
  .on {
    color: green;
  }

  .off {
    color: red;
  }
}
</style>
