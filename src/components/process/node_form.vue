<template>
  <div>
    <div class="ef-node-form">
      <div class="ef-node-form-header">
        编辑
      </div>
      <div class="ef-node-form-body">
        <Form :model="node"
              ref="dataForm"
              label-width="80px"
              v-show="type === 'node'">
          <FormItem label="类型">
            <Input v-model="node.type"
                   :disabled="true"></Input>
          </FormItem>
          <FormItem label="名称">
            <Input v-model="node.name"></Input>
          </FormItem>
          <!-- <FormItem label="left坐标">
                        <Input v-model="node.left"></Input>
                    </FormItem>
                    <FormItem label="top坐标">
                        <Input v-model="node.top"></Input>
                    </FormItem>
                    <FormItem label="ico图标">
                        <Input v-model="node.ico"></Input>
                    </FormItem> -->
          <FormItem>
            <Button icon="ios-close"
                    @click="deleteElement">删除</Button>
            <Button type="primary"
                    icon="ios-add"
                    @click="save">保存</Button>
          </FormItem>
        </Form>

        <Form :model="line"
              ref="dataForm"
              label-width="80px"
              v-show="type === 'line'">
          <FormItem label="条件">
            <Input v-model="line.label"></Input>
          </FormItem>
          <FormItem>
            <Button icon="ios-refresh-circle-outline">重置</Button>
            <Button type="primary"
                    icon="icon-add"
                    @click="saveLine">保存</Button>
          </FormItem>
        </Form>
      </div>
      <!--            <div class="el-node-form-tag"></div>-->
    </div>
  </div>

</template>

<script>
import { cloneDeep } from 'lodash'

export default {

  data () {
    return {
      visible: true,
      // node 或 line
      type: 'node',
      node: {},
      line: {},
      data: {}
    }
  },
  methods: {
    /**
     * 表单修改，这里可以根据传入的ID进行业务信息获取
     * @param data
     * @param id
     */
    nodeInit (data, id) {
      this.type = 'node'
      this.data = data
      data.nodeList.filter((node) => {
        if (node.id === id) {
          this.node = cloneDeep(node)
        }
      })
    },
    lineInit (line) {
      this.type = 'line'
      this.line = line
    },
    // 修改连线
    saveLine () {
      this.$emit('setLineLabel', this.line.from, this.line.to, this.line.label)
    },
    save () {
      this.data.nodeList.filter((node) => {

        if (node.id === this.node.id) {
          node.name = this.node.name
          node.left = this.node.left
          node.top = this.node.top
          this.$emit('repaintEverything')
        }
      })
    },
    // 删除激活的元素
    deleteElement () {
      this.$emit('delete')
    },
  }
}
</script>

<style>
.el-node-form-tag {
  position: absolute;
  top: 50%;
  margin-left: -15px;
  height: 40px;
  width: 15px;
  background-color: #fbfbfb;
  border: 1px solid rgb(220, 227, 232);
  border-right: none;
  z-index: 0;
}
</style>
