<template>
  <el-row type="flex" justify="space-between" align="middle" style="height: 40px;width: 100%">

    <el-col>
      <!-- 名称应该变成 对应的节点中的name -->
      <span>{{ treeNode.name }}</span>
    </el-col>

    <el-col :span="4">

      <el-row type="flex" justify="end">
        <!-- 两个内容 -->
        <el-col>{{ treeNode.manager }}</el-col>

        <el-col>
          <!-- 下拉菜单 element -->
          <!-- 下拉菜单的监听事件command -->
          <el-dropdown @command="operateDepts">
            <span>
              操作<i class="el-icon-arrow-down" />
            </span>
            <!-- 下拉菜单 -->
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="add" :disabled="!checkPermission('add-dept')">添加子部门</el-dropdown-item>
              <el-dropdown-item v-if="!isRoot" command="edit">编辑部门</el-dropdown-item>
              <el-dropdown-item v-if="!isRoot" command="del">删除部门</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-col>

      </el-row>

    </el-col>

  </el-row>
</template>

<script>
import { delDepartments } from '@/api/departments'
// 该组件需要对外开放属性 外部需要提供一个对象 对象里需要有name  manager
export default {
  // props可以用数组来接收数据 也可以用对象来接收
  // props: {   props属性: {  配置选项 }  }
  props: {
    //   定义一个props属性
    treeNode: {
      type: Object, // 对象类型
      required: true // 要求对方使用您的组件的时候 必须传treeNode属性 如果不传 就会报错
    },
    isRoot: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    operateDepts(type) {
      if (type === 'add') {
        // alert('789')
        this.$emit('addDepts', this.treeNode)
      } else if (type === 'edit') {
        // alert('456')
        this.$emit('editDepts', this.treeNode)
      } else {
        // 删除
        this.$confirm('确定删除该部门吗？').then(() => {
          return delDepartments(this.treeNode.id)
        }).then(() => {
        // 删除成功则
          this.$emit('delDepts')
          this.$message.success('删除部门成功')
        })
        // alert('123')
      }
    }
  }
}
</script>
