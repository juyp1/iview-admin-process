<template>
    <div class="container">
      <gantt class="left-container"
           :tasks="tasks"
           @task-updated="logTaskUpdate"
           @link-updated="logLinkUpdate"
           @task-selected="selectTask"></gantt>
    </div>
</template>

<script>
import Gantt from '@/components/vuegannt/index.vue'
export default {

  name: '',
  components: {
    Gantt
  },
  data () {
    return {
      tasks: {
        data: [
          { id: 1, text: '电视机组装', start_date: '2020-05-06', duration: 31, progress: 0.6 },
          { id: 2, text: '汽车组装', start_date: '2020-06-06', duration: 31, progress: 0 },
          { id: 3, text: '飞机组装', start_date: '2020-07-10', duration: 31, progress: 1 }
          // {id: 10, text: 'Task #2', start_date: '2017-04-04', duration: 3, progress: 0.4}
        ],
        links: [
          
        ]
      },
      selectedTask: null,
      messages: []
    }
  },
  filters: {
    toPercent (val) {
      if (!val) return '0'
      return Math.round((+val) * 100)
    },
    niceDate (obj) {
      return `${obj.getFullYear()} / ${obj.getMonth() + 1} / ${obj.getDate()}`
    }
  },
  methods: {
    selectTask (task) {
      this.selectedTask = task
      console.log('1111')
    },

    addMessage (message) {
      this.messages.unshift(message)
      if (this.messages.length > 40) {
        this.messages.pop()
      }
       console.log('2222')
    },

    logTaskUpdate (id, mode, task) {
      let text = (task && task.text ? ` (${task.text})` : '')
      let message = `Task ${mode}: ${id} ${text}`
      this.addMessage(message)
      console.log(id, task)
      this.tasks.data.push({ id: id, text: text, start_date: '2020-07-07', duration: 21, progress: 0.6 })
    },

    logLinkUpdate (id, mode, link) {
      let message = `Link ${mode}: ${id}`
      if (link) {
        message += ` ( source: ${link.source}, target: ${link.target} )`
      }
      this.addMessage(message)
    }
  }
}
</script>

<style  scoped>
  html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

.container {
  height: 100%;
  width: 100%;
}

.left-container {
  overflow: hidden;
  position: relative;
  height: 100%;
}

.right-container {
  border-right: 1px solid #cecece;
  float: right;
  height: 100%;
  width: 340px;
  box-shadow: 0 0 5px 2px #aaa;
  position: relative;
  z-index: 2;
}

.gantt-messages {
  list-style-type: none;
  height: 50%;
  margin: 0;
  overflow-x: hidden;
  overflow-y: auto;
  padding-left: 5px;
}

.gantt-messages > .gantt-message {
  background-color: #f4f4f4;
  box-shadow: inset 5px 0 #d69000;
  font-family: Geneva, Arial, Helvetica, sans-serif;
  font-size: 14px;
  margin: 5px 0;
  padding: 8px 0 8px 10px;
}

.gantt-selected-info {
  border-bottom: 1px solid #cecece;
  box-sizing: border-box;
  font-family: Geneva, Arial, Helvetica, sans-serif;
  height: 50%;
  line-height: 28px;
  padding: 10px;
}

.gantt-selected-info h2 {
  border-bottom: 1px solid #cecece;
}

.select-task-prompt h2 {
  color: #d9d9d9;
}
</style>
