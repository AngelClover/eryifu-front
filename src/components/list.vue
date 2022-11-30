<template>
  
  <v-container>
    <div>
      当前浏览器时间 : {{timecounter}}
    </div>
    <div>
      后端计算时间 : {{response.tm}}
    </div>
    <div>
      当前是否是开盘时间: {{isopen}}
    </div>
    <div align=right>
      <v-row align="end">
      <v-col
        class="d-flex"
        cols="12"
        sm="3"
      >
      <v-select
          :items="dateitems"
          label="数据日期"
          v-model="selectdate"
          @change="changedate(selectdate)"
        ></v-select>
      </v-col>
      </v-row>
    </div>

  <v-table >
    <thead>
      <tr>
        <th class="text-left">
          代码
        </th>
        <th class="text-left">
          名字
        </th>
        <th class="text-left">
          板块
        </th>
        <th class="text-left">
          放量比例
        </th>
        <th class="text-left">
          对比日期
        </th>
        <th class="text-left">
          今量
        </th>
        <th class="text-left">
          昨量
        </th>
        <th class="text-left">
          前一天比例
        </th>
        <th class="text-left">
          周量比例
        </th>
        <th class="text-left">
          当日涨幅
        </th>
        <th class="text-left">
         最近连续10穿73天数(-是下穿)
        </th>
        <th class="text-left">
          下日涨幅
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in content"
        :key="item.code"
      >
        <td>{{ item.code }}</td>
        <td><a :href="quotelink(item.code)" target="_blank">{{ item.name }}</a></td>
        <td>{{ item.bk }}</td>
        <td>{{ item.ratio }}</td>
        <td>{{ item.ckdate }}</td>
        <td>{{ item.now }}</td>
        <td>{{ item.last }}</td>
        <td :style="checkflag(item.llrf)">{{ item.llr }}</td>
        <td :style="checkflag(item.waf)">{{ item.war }}</td>
        <td :style="checkflag(item.detpf)">{{ item.detp }}</td>
        <td :style="checkflag(item.tosf)">{{ item.tos }}</td>
        <td >{{ item.next }}</td>
      </tr>
    </tbody>
  </v-table>

  </v-container>
</template>

<script>
  export default {
    data () {
      return {
          timecounter: null,
            response: {},
          content: [],
          isopen: false,
          dateitems: ["realtime", "2022-11-25", "2022-11-24"],
          selectdate: "realtime"
        }
    },
      mounted() {
          console.log("mounted")
          console.log(process.env.NODE_ENV)
          console.log(import.meta.env.VITE_BACKEND_ROOT)
          this.getList()
          this.getOpenStatus()
          this.getDateList()
          setInterval(() => {
          this.getList()
            }, 10000)
          setInterval(() => {
              this.timecounter = new Date()
            }, 1000)
          setInterval(() => {
          this.getOpenStatus()
            }, 10000)
        },
      computed : {
          datasource() {
              if (this.selectdate == "realtime"){
                  return "ss1"
                }else {
                    return "ss1/" + this.selectdate
                  }
            }
        },
      methods: {
          getDateList() {
              console.log("start get datelist")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "ss1list").then((res) => {
                  console.log(res.data)
                  //this.response = res.data
                  //this.content = this.response.data
                  this.dateitems = ["realtime"].concat(res.data.datelist)
                })
            },
          getList() {
              console.log("start get data")
              console.log("datasource :", this.datasource)
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + this.datasource).then((res) => {
                  console.log(res.data)
                  this.response = res.data
                  //obj = JSON.parse(this.response)
                  this.content = this.response.data
                })
            },
          getOpenStatus() {
              console.log("start get opentime")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "isopen").then((res) => {
                  console.log(res.data)
                  this.isopen = res.data.open
                  //obj = JSON.parse(this.response)
                })
            },
          checkflag(x) {
          if (x) {
              return "background-color: #FF0000"
            }else {
                return ""
              }
          },
          quotelink(x) {
              if (x.startsWith("688")){
                  return "http://quote.eastmoney.com/kcb/" + x + ".html"
                }else if (x.startsWith("60")){
                    return "http://quote.eastmoney.com/sh" + x + ".html"
                  }else if (x.startsWith("0")){
                      return "http://quote.eastmoney.com/sz" + x + ".html"
                    }else {
                        return ""
                      }
            },
          changedate(x) {
              console.log("receive date change ->", x)
              this.selectdate = x
            },
          changed() {
              console.log("changed received")
            }

        }

  }
</script>
