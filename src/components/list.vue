<template>
  
  <v-container>
    <div>
      当前浏览器时间 : {{timecounter}}
    </div>
    <div>
      后端计算时间 : {{response.tm}}
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
        }
    },
      mounted() {
          console.log("mounted")
          console.log(process.env.NODE_ENV)
          console.log(import.meta.env.VITE_BACKEND_ROOT)
          this.getList()
          setInterval(() => {
          this.getList()
            }, 100000)
          setInterval(() => {
              this.timecounter = new Date()
            }, 1000)
        },
      methods: {
          getList() {
              console.log("start get data")
              this.axios.get(import.meta.env.VITE_BACKEND_ROOT + "ss1").then((res) => {
                  console.log(res.data)
                  this.response = res.data
                  //obj = JSON.parse(this.response)
                  this.content = this.response.data
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
            }

        }

  }
</script>
