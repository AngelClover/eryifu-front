<template>
  
  <v-container>
    <div>
      当前浏览器时间 : {{timecounter}}
    </div>
    this is jobs
    <div>
      ss1
    </div>
    <v-btn @click=triggerss1> ss1触发 </v-btn>
    <div>
      {{ss1}}
    </div>

    <div>
      prefill
    </div>
    <v-btn @click=triggerprefill> prefill触发 </v-btn>
    <div>
      {{prefill}}
    </div>

  </v-container>
</template>

<script>
  export default {
    data () {
      return {
          timecounter: null,
            ss1: {},
          prefill: {},
        }
    },
      mounted() {
          console.log("mounted")
          console.log(process.env.NODE_ENV)
          console.log(import.meta.env.VITE_BACKEND_ROOT)
          this.getss1()
          this.getprefill()
          setInterval(() => {
          this.getss1()
          this.getprefill()
            }, 10000)
          setInterval(() => {
              this.timecounter = new Date()
            }, 1000)
        },
      methods: {
          getss1() {
              console.log("start get job_status_ss1")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "db/job_status_ss1").then((res) => {
                  console.log(res.data)
                  this.ss1 = res.data
                })
            },
          triggerss1() {
              console.log("start trigger ss1")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "job/ss1").then((res) => {
                  console.log(res.data)
                })
            },
          getprefill() {
              console.log("start get job_status_prefill")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "db/job_status_prefill").then((res) => {
                  console.log(res.data)
                  this.prefill = res.data
                })
            },
          triggerprefill() {
              console.log("start trigger prefill")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "job/prefill").then((res) => {
                  console.log(res.data)
                })
            },

        }

  }
</script>
