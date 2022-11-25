<template>
  <v-app>
    <v-main>
      <component :is="currentView" />
    </v-main>
  </v-app>
</template>

<script >
  import List from '@/components/list.vue'
  import Jobs from '@/components/jobs.vue'
export default {
  data() {
    return {
      currentPath: window.location.hash
    }
  },
  computed: {
    currentView() {
      console.log("Angel ", this.currentPath)
      //pa=this.currentPath.slice(1)
      //console.log("Angel pa:", this.currentPath.slice(1))
      if (this.currentPath.slice(1) == "jobs") return Jobs
      return List
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
      this.currentPath = window.location.hash
      console.log("Angel ", this.currentPath, " in event listener")
      console.log("Angel location:", window.location, " in event listener")
    })
  }
}
</script>
