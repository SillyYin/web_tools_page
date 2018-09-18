<template>
  <div class="hello">
    <div id="main" style="width: 1200px;height:400px; margin-top: 25px"></div>
    <div style="margin-top: 25px">
      <el-radio v-model='radio' label="true" border @change="change">正确</el-radio>
      <el-radio v-model='radio' label="false" border @change='change'>错误</el-radio>
    </div>
    <div style="margin-top: 25px">
      <el-button @click="pre_page">上一条</el-button>
      {{index + 1}} / {{data.length}}
      <el-button @click="next_page">下一条</el-button>
      <el-button type="primary" @click="save">保存</el-button>
    </div>
  </div>
</template> 

<script>
import $ from "jquery";
import echarts from "echarts";

let myChart = null;
let _data = {};

export default {
  name: "HelloWorld",
  data() {
    return {
      data: [],
      index: 0,
      boolList: [],
      radio: ""
    };
  },
  mounted() {
    myChart = echarts.init(document.getElementById("main"));
    const _this = this;
    $.post("http://localhost:8000/initialize", {}, function(res) {
      let resObj = JSON.parse(res);
      if (resObj.status === "FAIL") {
        alert("加载错误，请联系管理员！");
      } else {
        //_this.data = resObj.data.map(item => JSON.parse(item));
        //_data = resObj.data.map(item => JSON.parse(item));
        _this.data = resObj.data
        _data = resObj.data
        _this.boolList = resObj.bool;
        _this.radio = _this.boolList[_this.index]
        let option = {
          tooltip: {
            trigger: "item"
            //triggerOn: 'mousemove'
          },
          series: [
            {
              type: "tree",
              data: [JSON.parse(_data[_this.index])],
              left: "5%",
              right: "5%",
              top: "8%",
              bottom: "20%",
              symbol: "emptyCircle",
              orient: "vertical",
              expandAndCollapse: true,
              label: {
                normal: {
                  position: "top",
                  rotate: 0,
                  verticalAlign: "middle",
                  align: "left",
                  fontSize: 14
                }
              },
              leaves: {
                label: {
                  normal: {
                    position: "bottom",
                    rotate: 0,
                    verticalAlign: "middle",
                    align: "center"
                  }
                }
              },
              animationDurationUpdate: 750
            }
          ]
        };

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
      }
    });
  },
  methods: {
    next_page() {
      const _this = this;
      let currentIndex = _this.index;
      if (currentIndex >= _data.length-1) {
        alert("页码超出范围");
      }else{
      _this.index += 1;
      currentIndex++;
      
      _this.radio = _this.boolList[currentIndex]
      let option = {
        tooltip: {
          trigger: "item"
          //triggerOn: 'mousemove'
        },
        series: [
          {
            type: "tree",
            data: [JSON.parse(_data[currentIndex])],
            left: "5%",
            right: "5%",
            top: "8%",
            bottom: "20%",
            symbol: "emptyCircle",
            orient: "vertical",
            expandAndCollapse: true,
            label: {
              normal: {
                position: "top",
                rotate: 0,
                verticalAlign: "middle",
                align: "left",
                fontSize: 14
              }
            },
            leaves: {
              label: {
                normal: {
                  position: "bottom",
                  rotate: 0,
                  verticalAlign: "middle",
                  align: "center"
                }
              }
            },
            animationDurationUpdate: 750
          }
        ]
      };
      myChart.setOption(option);
      }
    },

    pre_page() {
      const _this = this;
      let dataLength = _data.length;
      let currentIndex = _this.index;
      if (currentIndex <= 0) {
        alert("页码超出范围");
      }else{
      _this.index -= 1;
      currentIndex--;
      _this.radio = _this.boolList[currentIndex]
      let option = {
        tooltip: {
          trigger: "item"
          //triggerOn: 'mousemove'
        },
        series: [
          {
            type: "tree",
            data: [JSON.parse(_data[currentIndex])],
            left: "5%",
            right: "5%",
            top: "8%",
            bottom: "20%",
            symbol: "emptyCircle",
            orient: "vertical",
            expandAndCollapse: true,
            label: {
              normal: {
                position: "top",
                rotate: 0,
                verticalAlign: "middle",
                align: "left",
                fontSize: 14
              }
            },
            leaves: {
              label: {
                normal: {
                  position: "bottom",
                  rotate: 0,
                  verticalAlign: "middle",
                  align: "center"
                }
              }
            },
            animationDurationUpdate: 750
          }
        ]
      };
      myChart.setOption(option);
      }
    },

    judge() {},

    save() {
      const _this = this;
      $.post(
        "http://localhost:8000/save",
        {'bool_list': JSON.stringify(_this.boolList)},
        function(res){
          let resObj = JSON.parse(res);
          if (resObj.status === 'SUCCESS'){
            alert('保存成功')
          }else{
            alert('保存失败，请联系管理员')
          }
        }
      )
    },

    change() {
      const _this = this
      _this.boolList[_this.index] = _this.radio
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
