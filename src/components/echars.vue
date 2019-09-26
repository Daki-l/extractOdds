<template>
  <div class="echars-box" id="myChart">
      {{date}}
  </div>
</template>

<script>
export default {
    name: 'Echars',
    data() {
        return {
            date: 123,
            time: 0,
            tooltipData: ['总概率', '临时总概率', '四星概率', '五星概率'],
            xData: [],
            allProxxx: [],
            momAllProxxx: [],
            fourProxxx: [],
            fiveProxxx: []
        };
    },
    mounted() {
        this.drawLine();
    },
    methods: {
        drawLine() {
            // 基于准备好的dom，初始化echarts实例
            let myChart = this.$echarts.init(document.getElementById('myChart'))
            // 绘制图表
            myChart.setOption({
                tooltip: {},
                xAxis: {
                    type: 'category',
                    data: this.xData
                },
                yAxis: {
                    type: 'value'
                },
                series: [{
                    name: this.tooltipData[0],
                    type: 'line',
                    data: this.allProxxx,
                    lineStyle: { color: 'black' }
                },
                {
                    name: this.tooltipData[1],
                    type: 'line',
                    data: this.momAllProxxx,
                    lineStyle: { color: '#2200ff' }
                },
                {
                    name: this.tooltipData[2],
                    type: 'line',
                    data: this.fourProxxx,
                    lineStyle: { color: '#44ff00' }
                },
                {
                    name: this.tooltipData[3],
                    type: 'line',
                    data: this.fiveProxxx,
                    lineStyle: { color: 'red' }
                }]
            });
        },
        addNewData(data, allproba) {
            let momAllProba = parseFloat(allproba).toFixed(2);
            let momAllrPro = '';
            let momFourPro = this.getPro(data, 'four');
            let momFivePro = this.getPro(data, 'five');
            momAllrPro = parseInt(momFourPro + momFivePro);
            this.time++;
            this.xData.push(this.time);
            this.fourProxxx.push(momFourPro);
            this.fiveProxxx.push(momFivePro);
            this.momAllProxxx.push(momAllrPro);
            this.allProxxx.push(momAllProba);
            this.drawLine();
        },
        // 获取概率
        getPro(data, type) {
            var r = 0;
            var k = 0;
            var len = data.length;
            for (var i = 0; i < data.length; i++) {
                if (data[i].type === type) {
                    k++;
                }
            }
            r = k / len * 100;
            return !!r ? r : 0;
        },
        clear() {
            this.time = 0;
            this.xData = [];
            this.allProxxx = [];
            this.momAllProxxx = [];
            this.fourProxxx = [];
            this.fiveProxxx = [];
            this.drawLine();
        }
    }
}
</script>

<style>
.echars-box {
    width: 800px;
    height: 300px;
}
</style>
