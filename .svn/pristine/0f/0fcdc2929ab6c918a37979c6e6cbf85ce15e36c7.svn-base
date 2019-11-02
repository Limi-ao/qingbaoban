<template>
  <div class="container">
    <iframe src="/static/pages/index.html" frameborder="0" width="100%" style="height:calc(100vh - 64px)"></iframe>
    <div class="leftTop">
      <el-button :type="isSelected?'primary':'info'" size="small" @click="changeEcharts(1)">车流量</el-button>
      <el-button :type="isSelected?'info':'primary'" size="small" @click="changeEcharts(2)">气象仪</el-button>
      <div id="echart" style="position:absolute;left:0px;top:40px;width:320px;height:220px;border:1px solid #409EFF"></div>
    </div>
    <div class="leftBottom">
    
    </div>  
  </div>
</template>

<script>
const echarts = require('echarts/lib/echarts')
require('echarts/lib/component/tooltip')
require('echarts/lib/chart/line');
require('echarts/lib/component/grid')
require('echarts/lib/component/title')
require('echarts/lib/component/legend')
  export default {
    data() {
      return {
        isSelected:true,
        myChart:"",
        option:{
          grid:{
            left:50,
            right:15,
            bottom:30
          },
          tooltip:{
            trigger:'axis',
            showContent:true
          },
          legend:{
            data:['秀山大桥','官山大桥'],
            textStyle:{color:'#409EFF'}
          },
          xAxis:{
            type:'category',
            axisLine:{
              lineStyle:{color:'#409EFF'}
            },
            data:['周一','周二','周三','周四','周五','周六','周日']
          },
          yAxis:{
            type:'value',
            name:'车流量',
            axisLine:{
              lineStyle:{color:'#409EFF'},
              symbol:['none','arrow'],
            },
            splitLine: {
              show: true,
              lineStyle:{
                color:"#409EFF",
                  width: 1,
                  type: 'solid'
              }
            },
          },
          series:[
            {
              name:'秀山大桥',
              data:[1500,500,100,1200,300,500,200],
              type:'line',
              smooth:true,
              symbol:'circle',
              symbolSize:5,
              showSymbol:true,
              color:'#00ffff',
            },
            {
              name:'官山大桥',
              data:[100,200,300,400,500,1200,2200],
              type:'line',
              smooth:true,
              symbol:'circle',
              symbolSize:5,
              showSymbol:true,
              color:'#ffff00',
            }
          ]
        },
        optionTwo:{
          tooltip:{
            trigger:'axis',
            showContent:true
          },
          legend:{
            data:['秀山大桥','官山大桥'],
            textStyle:{color:'#0099ff'}
          },
          xAxis:{
            type:'category',
            axisLine:{
              lineStyle:{color:'#0099ff'}
            },
            data:["1","2","3","4","5","6","7"]
          },
          yAxis:{
            type:'value',
            name:'温度',
            axisLine:{
              lineStyle:{color:'#0099ff'},
              symbol:['none','arrow'],
            },
            splitLine: {
              show: true,
              lineStyle:{
                color:"#0099ff",
                  width: 1,
                  type: 'solid'
              }
            },
          },
          series:[{
              name:'秀山大桥',
              data:[22,23,23.5,25,34,26,28],
              type:'line',
              smooth:true,
              symbol:'circle',
              symbolSize:5,
              showSymbol:true,
              color:'#00ffff',
            },
            {
              name:'官山大桥',
              data:[34,31,27,23,26,34,33],
              type:'line',
              smooth:true,
              symbol:'circle',
              symbolSize:5,
              showSymbol:true,
              color:'#ffff00',
            }
          ]
        }
      }
    },
    mounted(){
      window.selectMap = this.selectMap
      this.myChart = echarts.init(document.getElementById('echart'))
      this.myChart.setOption(this.option);
    },
    methods: {
      changeEcharts(num){
        if(num === 1){
          this.isSelected = true
          this.myChart.setOption(this.option)
        }else{
          this.isSelected = false
          this.myChart.setOption(this.optionTwo)
        }
      },
      selectMap(name){
        let _this = this;
        switch(name){
          case 'shuidao':
            _this.$router.push('/suidao');
            break;
          case 'gsdq':
            _this.$router.push('/guanshan');
            break;
          default:
            _this.$router.push('/xiushan')
        }
      }     
    },
  }
</script>

<style scoped>
.leftTop{
  position: absolute;
  left:20px;
  top:80px;
}
.leftBottom{
  position:absolute;
  left: 20px;
  bottom: 50px;
  width: 250px;
  height:250px;
  border: 1px solid #0099ff;
}
</style>
