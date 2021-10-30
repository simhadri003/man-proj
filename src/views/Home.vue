<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @update-ref-det="updateRefrees"
    :matches="matches"
  />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      matches: [],
    }
  },
  methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        })

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('Error deleting task')
      }
    },
    async updateRefrees(id, refs) {
      const matchToUpdate = await this.fetchTask(id)
      const a = matchToUpdate['refrees']
      const b = refs.split(',')
      a.push.apply(a, b)

      const updMatch = { ...matchToUpdate, refrees: a }

      const res = await fetch(`api/matches/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updMatch),
      })

      const data = await res.json()

      this.matches = this.matches.map((match) =>
        match.id === id ? { ...match, refrees: a } : match
      )
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })

      const data = await res.json()

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
    async fetchTasks() {
      const res = await fetch('api/matches')

      const data = await res.json()
      console.log("data", data)

      return data
    },
    async fetchTask(id) {
      const res = await fetch(`api/matches/${id}`)

      const data = await res.json()

      return data
    },
  },
  async created() {
    this.matches = await this.fetchTasks()
  },
}
</script>
