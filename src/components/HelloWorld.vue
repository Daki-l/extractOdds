<template>
  <div class="hello">
    <div class="btn-list">
      <button @click="btnClick">抽取</button>
      <button @click="btnClickTen">抽取10次</button>
      <button @click="resetClick">加载小黄书概率</button>
      <button @click="resetClick2">加载光暗概率</button>
      <button @click="automatic">自动抽取5抽每次</button>
      <button @click="stop">停止</button>
      <button @click="clearClick">clear</button>
    </div>
    <div class="probxx-box">
        <p>概率 {{ parseFloat(fiveProbxx) + parseFloat(fourProbxx) + '%'}}</p>
        总数&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{getArr.length}} 个
        <p>
            <span v-if="isYellow">(0.5%)</span>
            <span v-if="!isYellow">(0.3%)</span>
            5星
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            {{fiveProbxx + '%'}}

        </p>
        <p>
            <span v-if="isYellow">(8.0%)</span>
            <span v-if="!isYellow">(6.0%)</span>
            4星
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            {{fourProbxx+ '%'}}
        </p>
    </div>
    <div class="result-box" id="box">
        <h4>抽取结果</h4>
        <div class="result-item">
            <span v-for="(data, index) in getArr" :key="index" :class="data.type">{{data.name}}</span>
        </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data() {
        return {
            msg: '',
            isYellow: true,
            time: '',
            temArr: [],
            getArr: []
        };
    },
    mounted() {
        this.init();
        this.rloading();
    },
    methods: {
        init() {
            var arr = [];
            for (var i = 0; i < 1000; i++) {
                arr.push({
                    name: '三星',
                    id: i,
                    type: 'three'
                });
            }
            this.temArr = arr;
            this.rloading();
        },
        btnClick() {
            var idd = parseInt(Math.random() * 1000);
            var arr = this.temArr;
            this.getArr.push({
                name: arr[idd].name,
                type: arr[idd].type,
                id: arr[idd].id
            });
        },
        // 抽10次
        btnClickTen() {
            var arr = this.temArr;
            for (var i = 0; i < 10; i ++) {
                var idd = parseInt(Math.random() * 1000);
                this.getArr.push({
                    name: arr[idd].name,
                    type: arr[idd].type,
                    id: arr[idd].id
                });
            }
        },
        // 加载
        rloading() {
            var fiveArrIndexArr = [];
            var fourArrIndexArr = [];
            // 生成5星数据
            for (var i = 0; i < 5; i++) {
                fiveArrIndexArr[i] = parseInt(Math.random() * 1000);
                for (var j = 0; j < i; j++) {
                    if (fiveArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
                
            }
            // 生成4星数据
            for (var i = 0; i < 80; i++) {
                fourArrIndexArr[i] = parseInt(Math.random() * 1000);
                for (var j = 0; j < i; j++) {
                    if (fourArrIndexArr[i] === fourArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
                for (var j = 0; j < 5; j++) {
                    if (fourArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
            }

            for (var i = 0; i < fiveArrIndexArr.length; i++) {
                this.temArr[fiveArrIndexArr[i]] = {
                    name: '五星',
                    id: i,
                    type: 'five'
                };
            }
            for (var i = 0; i < fourArrIndexArr.length; i++) {
                this.temArr[fourArrIndexArr[i]] = {
                    name: '四星',
                    id: i,
                    type: 'four'
                };
            }
        },
        // 加载
        rloading2() {
            var fiveArrIndexArr = [];
            var fourArrIndexArr = [];
            // 生成5星数据
            for (var i = 0; i < 3; i++) {
                fiveArrIndexArr[i] = parseInt(Math.random() * 1000);
                for (var j = 0; j < i; j++) {
                    if (fiveArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
                
            }
            // 生成4星数据
            for (var i = 0; i < 60; i++) {
                fourArrIndexArr[i] = parseInt(Math.random() * 1000);
                for (var j = 0; j < i; j++) {
                    if (fourArrIndexArr[i] === fourArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
                for (var j = 0; j < 5; j++) {
                    if (fourArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
            }

            for (var i = 0; i < fiveArrIndexArr.length; i++) {
                this.temArr[fiveArrIndexArr[i]] = {
                    name: '五星',
                    id: i,
                    type: 'five'
                };
            }
            for (var i = 0; i < fourArrIndexArr.length; i++) {
                this.temArr[fourArrIndexArr[i]] = {
                    name: '四星',
                    id: i,
                    type: 'four'
                };
            }
        },
        // 小黄书概率
        resetClick() {
            this.isYellow = true;
            this.clearClick();
            this.init();
            this.rloading();
        },
        // 光暗概率
        resetClick2() {
            this.isYellow = false;
            this.clearClick();
            this.init();
            this.rloading2();
        },
        myErval() {
            
        },
        // 自动抽取每次10抽
        automatic(type) {
            var arr = this.temArr;
            var mi = 0;
            this.time = setInterval(() => {
                mi++;
                for (var i = 0; i < 10; i ++) {
                    if (mi > 1000) {
                        console.log(i)
                        clearInterval(this.time);
                        return;
                    }
                    var idd = parseInt(Math.random() * 1000);
                    this.getArr.push({
                        name: arr[idd].name,
                        type: arr[idd].type,
                        id: arr[idd].id
                    });
                }
            }, 300)
        },
        stop() {
            clearInterval(this.time);
        },
        // 清除
        clearClick() {
            this.getArr = [];
        },
    },
    computed: {
        fiveProbxx() {
            var r = 0;
            var data = this.getArr;
            var len = data.length;
            var k = 0;
            for (var i = 0; i < data.length; i++) {
                if (data[i].type === 'five') {
                    k++;
                }
            }
            r = k / len * 100;
            return r.toFixed(2);
        },
        fourProbxx() {
            var r = 0;
            var data = this.getArr;
            var len = data.length;
            var k = 0;
            for (var i = 0; i < data.length; i++) {
                if (data[i].type === 'four') {
                    k++;
                }
            }
            r = k / len * 100;
            return r.toFixed(2);
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.btn-list{
    overflow: hidden;
    position: absolute;
    left: 10%;
    width: 250px;
    line-height: 30px;
}
.btn-list button{
    margin-left: 5px;
}
.probxx-box{
    position: absolute;
    left: 15%;
    top: 170px;
}

.result-box{
    float: right;
    width: 60%;
    border: solid 1px black;
    height: 800px;
    overflow-y: auto;
    position: absolute;
    right: 20px;
    top: 20px;
}
.result-item{
    text-align: left;
}
.result-item span{
    display: inline-block;
    width: 100px;
    text-align: center;
    margin-bottom: 10px;
}
.four{
    color: #0067ed;
}
.five{
    color: red;
}
</style>
