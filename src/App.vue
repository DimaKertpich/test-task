<template>
  <div id="app">
      <dv-task></dv-task>
  </div>
</template>

<script>

import task from "./components/test.vue"

export default {
  name: 'App',
  components: {
    "dv-task" : task
  },
}
</script>
