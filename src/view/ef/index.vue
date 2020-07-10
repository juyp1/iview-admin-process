<template>
  <div class="flowcontainer">
    <flow-panel :menusInfo='menuList'
                @cleardata="handleclear"
                :loadinfo='data_Z'></flow-panel>
  </div>
</template>

<script>
import FlowPanel from '../../components/process/panel'
export default {
  components: {
    FlowPanel
  },
  name: 'ef',
  data () {
    return {
      data_Z: {
        name: '新建流程',
        nodeList: [

        ],
        lineList: [
          {
            from: 'nodeA',
            to: 'nodeB',
            label: '直线,自定义线样式,固定锚点',
            connector: 'Straight',
            anchors: ['Top', 'Bottom'],
            paintStyle: { strokeWidth: 2, stroke: '#1879FF' }
          }
        ]
      },
      // 传入的参数
      menuList: [
        {
          id: '1',
          type: 'group',
          name: '工艺路线',
          ico: 'el-icon-video-play',
          type: '1',
          open: true,
          children: [
            {
              id: '1',
              type: 'task1',
              name: '工序1',
              content: '我是一个粉刷匠',
              ico: 'el-icon-time',
              // 自定义覆盖样式
              state: 'warning',
              style: {}
            }, {
              id: '2',
              type: '2',
              name: '工序2',
              content: '刷子费五毛',
              ico: 'el-icon-s-fold',
              // 自定义覆盖样式
              style: {}
            },
            {
              id: '3',
              type: '3',
              name: '工序3',
              content: '哎呀我的小鼻子',
              ico: 'el-icon-odometer',
              // 自定义覆盖样式
              style: {

              }
            },
            {
              id: '4',
              type: '4',
              name: '工序x',
              content: '变得真可爱',
              ico: 'el-icon-odometer',
              state: 'error',
              // 自定义覆盖样式
              style: {
                'color': 'red'
              }
            }
          ]
        }

      ]
    }
  },
  methods: {
    handleclear () {

    }
  }
}
</script>

<style   scoped>
.flowcontainer {
  background: #ffffff;
}
</style>
