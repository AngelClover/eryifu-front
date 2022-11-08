<template>
  
  <v-container>
    <div>
      frond end time : {{timecounter}}
    </div>
  <v-table >
    <thead>
      <tr>
        <th class="text-left">
          Code
        </th>
        <th class="text-left">
          Name
        </th>
        <th class="text-left">
          bk
        </th>
        <th class="text-left">
          Ratio
        </th>
        <th class="text-left">
          lastdate
        </th>
        <th class="text-left">
          amount
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in content"
        :key="item.code"
      >
        <td>{{ item.code }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.bk }}</td>
        <td>{{ item.ratio }}</td>
        <td>{{ item.ckdate }}</td>
        <td>{{ item.amount }}</td>
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
          desserts: [
              {
                  name: 'Frozen Yogurt',
                  calories: 159,
                },
              {
                  name: 'Ice cream sandwich',
                  calories: 237,
                },
              {
                  name: 'Eclair',
                  calories: 262,
                },
              {
                  name: 'Cupcake',
                  calories: 305,
                },
              {
                  name: 'Gingerbread',
                  calories: 356,
                },
              {
                  name: 'Jelly bean',
                  calories: 375,
                },
              {
                  name: 'Lollipop',
                  calories: 392,
                },
              {
                  name: 'Honeycomb',
                  calories: 408,
                },
              {
                  name: 'Donut',
                  calories: 452,
                },
              {
                  name: 'KitKat',
                  calories: 518,
                },
            ],
            response: {},
          content: [],
        }
    },
      mounted() {
          console.log("mounted")
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
              this.axios.get("http://localhost:8080/ss1").then((res) => {
                  console.log(res.data)
                  this.response = res.data
                  //obj = JSON.parse(this.response)
                  //console.log(obj)
                  this.content = this.response.data
                })
            }
        }

  }
</script>
