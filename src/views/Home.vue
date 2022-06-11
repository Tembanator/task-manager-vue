<template>
  <div class="home">
    <div class="tasks" v-if="tasks.length">
      <div v-for="task in tasks">
        <SingleTask :task="task" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleTask from '../components/SingleTask.vue'
export default {
  name: 'Home',
  components: {
    SingleTask
  },
  data() {
    return {
      tasks: []
    }
  },
  mounted() {
    fetch('http://localhost:3000/tasks')
    .then((res) => res.json())
    .then((data) => this.tasks = data)
    .catch((err) => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.tasks = this.tasks.filter((task) => task.id != id)
    },
    handleComplete(id) {
      let t = this.tasks.find((task) => task.id === id)
      t.complete = !t.complete
    }
  }
}
</script>
