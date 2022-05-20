<template>
  <header>
    <h1>{{ title }}</h1>
    <Button @btn-click="$emit('toggle-add-task')"
            v-show="homePage"
            :text="showAddTask ? 'Close' : 'Add Task'"
            :color="showAddTask ? 'red' : 'green'"/>
  </header>
</template>

<script>
import Button from "@/components/Button"

export default {
  name: "Header",
  components: {Button},
  props: {
    title: {
      type: String,
      default: 'Hello World'
    },
    showAddTask: Boolean
  },
  computed: {
    homePage() {
      return this.$route.path === '/'
    }
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>