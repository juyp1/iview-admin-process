
<template>
  <div ref="gantt"></div>
</template>

<script>
import 'dhtmlx-gantt'
import 'dhtmlx-gantt/codebase/locale/locale_cn'
export default {
  name: 'gantt',
  props: {
    tasks: {
      type: Object,
      default () {
        return { data: [], links: [] }
      }
    }
  },

  methods: {
    $_initGanttEvents: function () {
      if (gantt.$_eventsInitialized) { return }
      gantt.attachEvent('onTaskSelected', (id) => {
        let task = gantt.getTask(id)
        this.$emit('task-selected', task)
      })

      gantt.attachEvent('onAfterTaskAdd', (id, task) => {
        this.$emit('task-updated', id, 'inserted', task)
        task.progress = task.progress || 0
        if (gantt.getSelectedId() == id) {
          this.$emit('task-selected', task)
        }
      })

      gantt.attachEvent('onAfterTaskUpdate', (id, task) => {
        this.$emit('task-updated', id, 'updated', task)
      })

      gantt.attachEvent('onAfterTaskDelete', (id) => {
        this.$emit('task-updated', id, 'deleted')
        if (!gantt.getSelectedId()) {
          this.$emit('task-selected', null)
        }
      })

      gantt.attachEvent('onAfterLinkAdd', (id, link) => {
        this.$emit('link-updated', id, 'inserted', link)
      })

      gantt.attachEvent('onAfterLinkUpdate', (id, link) => {
        this.$emit('link-updated', id, 'updated', link)
      })

      gantt.attachEvent('onAfterLinkDelete', (id, link) => {
        this.$emit('link-updated', id, 'deleted')
      })
      gantt.$_eventsInitialized = true
    }
  },

  mounted () {
    gantt.config.scale_unit = 'month'
    gantt.config.date_scale = '%Y-%m'
    gantt.config.xml_date = '%Y-%m-%d %H:%i'
    gantt.config.scale_height = 50
    // gantt.config.drag_resize = false// 两边不可拖动
    // gantt.config.readonly = true// 只读模式
    gantt.config.columns = [
      { name: 'text', label: '任务名称', width: '*', tree: false },
      { name: 'start_date', label: '开始时间', align: 'center', tree: false },
      { name: 'duration', label: '持续天数', align: 'center', tree: false }
    // {name:"add",        label:"",           width:44 }
    ]
    gantt.config.subscales = [
		  { unit: 'day', step: 1, date: '%m-%d' }
    ]
    this.$_initGanttEvents()

    gantt.init(this.$refs.gantt)
    gantt.parse(this.$props.tasks)
  }
}
</script>

<style>
  @import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";

</style>
