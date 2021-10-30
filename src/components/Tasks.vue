<template>
  <div class="task-fp">
    <h2>Id</h2>
    <h2>Match</h2>
    <h2>Level</h2>
    <h2>Refrees</h2>
    <h2>Edit</h2>
  </div>
  <div :key="match.id" v-for="match in matches">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @update-ref-det="$emit('update-ref-det', id, refs)"
      :match="match"
      header="false"
    />
  </div>
</template>

<script>
import Task from './Task'

export default {
  name: 'Tasks',
  props: {
    matches: Array,
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-reminder'],
}
</script>
