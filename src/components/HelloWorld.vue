<template>
  <div class="hello">
    <button>上一条</button>
    <div id="main" style="width: 1200px;height:400px;"></div>
    <button @click="next_page">下一条</button>
  </div>
</template>

<script>
import $ from 'jquery'
import echarts from 'echarts'

let myChart = null;
console.log(myChart)
let _data = {}

export default {
  name: 'HelloWorld',
  data () {
    return {
      data: [],
      index: 0,
    };
  },
  mounted(){
    myChart = echarts.init(document.getElementById('main'))
    const _this = this;
    $.post(
      "http://localhost:8000/initialize",
      {},
      function(res){
        let resObj = JSON.parse(res);
        _this.data = resObj.data.map(item=>JSON.parse(item))
        _data = resObj.data.map(item=>JSON.parse(item))
        let option = {
            tooltip: {
                trigger: 'item',
                //triggerOn: 'mousemove'
            },
            series: [{
                type: 'tree',
                data: [_data[_this.index]],
                left: '5%',
                right: '5%',
                top: '8%',
                bottom: '20%',
                symbol: 'emptyCircle',
                orient: 'vertical',
                expandAndCollapse: true,
                label: {
                    normal: {
                        position: 'top',
                        rotate: 0,
                        verticalAlign: 'middle',
                        align: 'left',
                        fontSize: 11
                    }
                },
                leaves: {
                    label: {
                        normal: {
                            position: 'bottom',
                            rotate: 0,
                            verticalAlign: 'middle',
                            align: 'center'
                        }
                    }
                },
                animationDurationUpdate: 750
            }]
        }

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
      }
    )
  },
  methods: {
    next_page() {
      let currentIndex = this.index;
      this.index += 1;
      currentIndex++;
        let option = {
            tooltip: {
                trigger: 'item',
                //triggerOn: 'mousemove'
            },
            series: [{
                type: 'tree',
                data: [_data[currentIndex]],
                left: '5%',
                right: '5%',
                top: '8%',
                bottom: '20%',
                symbol: 'emptyCircle',
                orient: 'vertical',
                expandAndCollapse: true,
                label: {
                    normal: {
                        position: 'top',
                        rotate: 0,
                        verticalAlign: 'middle',
                        align: 'left',
                        fontSize: 11
                    }
                },
                leaves: {
                    label: {
                        normal: {
                            position: 'bottom',
                            rotate: 0,
                            verticalAlign: 'middle',
                            align: 'center'
                        }
                    }
                },
                animationDurationUpdate: 750
            }]
        }
        myChart.setOption(option);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
