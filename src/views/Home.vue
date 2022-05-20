<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask"/>
  </div>
  <Tasks @toggle-reminder="toggleReminder"
         @delete-task="deleteTask"
         :tasks="tasks"/>
</template>

<script>
import Tasks from "@/components/Tasks"
import AddTask from "@/components/AddTask"

export default {
  name: "Home",
  props: {
    showAddTask: Boolean
  },
  components: {
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    addTask(task) {
      fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })
      .then(res => res.json())
      .then((result) => {
        this.tasks = [...this.tasks, result]
      })
    },
    deleteTask(id) {
      if (confirm('Are you sure?')) {
        fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        })
        .then(() => {
          this.tasks = this.tasks.filter(task => task.id !== id)
        })
        .catch(() => {
          alert('Error deleting task')
        })
      }
    },
    toggleReminder(id) {
      this.fetchTask(id)
          .then(res => {
            return {...res, reminder: !res.reminder}
          })
          .then(res => {
            fetch(`api/tasks/${id}`, {
              method: 'PUT',
              headers: {
                'Content-type': 'application/json'
              },
              body: JSON.stringify(res)
            })
            .then(res => res.json())
            .then(data => {
                  console.log(data)
                  this.tasks = this.tasks.map(task => task.id === id ? {...task, reminder: data.reminder} : task)
                }
            )
          })
    },
    fetchTasks() {
      return fetch('api/tasks')
      .then(res => res.json())
    },
    fetchTask(id) {
      return fetch(`api/tasks/${id}`)
      .then(res => res.json())
    },
  },
  created() {
    this.fetchTasks()
        .then(task => this.tasks = task)
  }
}
</script>

<style scoped>

</style>