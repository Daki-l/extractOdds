<template>
    <div class="hello">
        <div class="btn-list">
            <button @click="btnClick">抽取</button>
            <button @click="btnClickTen">抽取10次</button>
            <button @click="resetClick">小黄书概率</button>
            <button @click="resetClick2">光暗概率</button>
            <button @click="automatic">自动抽取</button>
            <button @click="stop">停止</button>
            <button @click="clearClick">clear</button>
            <button @click="showResult = !showResult">显示结果</button>
        </div>
        <echars ref="myEchars"
            @momAllrPro="momAllrPro"
        ></echars>
        <div class="contant">
            <div class="left-box" v-if="showResult">
                <div class="result-box" id="box">
                    <div class="result-item">
                        <p class="result-title">抽取结果</p>
                        <span v-for="(data, index) in getArr" :key="index" :class="data.type">{{data.name}}</span>
                    </div>
                </div>
            </div>
            <div class="right-box">
                <div class="show-hightProbability">
                    <span class="text" v-for="(d, i) in hightProbabilities" :key="i">
                        done{{d}}
                    </span>
                </div>
                <div class="probxx-box">
                    <p>概率 {{ allproba()  + '%' }}</p>
                    总数<br>{{getArr.length}} 个
                    <p>
                        <span v-if="isYellow">(0.5%)</span>
                        <span v-if="!isYellow">(0.3%)</span>
                        5星
                        <br>
                        {{ !!fiveProbxx ? fiveProbxx : 0 + '%'}}
                    </p>
                    <p>
                        <span v-if="isYellow">(8.0%)</span>
                        <span v-if="!isYellow">(6.0%)</span>
                        4星
                        <br>
                        {{fourProbxx+ '%'}}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Echars from './echars';
export default {
    name: 'HelloWorld',
    components: {
        Echars
    },
    data() {
        return {
            msg: '',
            isYellow: true,
            showResult: false,
            time: '',
            clickColum: 0,
            temArr: [],
            getArr: [],
            hightProbabilities: []
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
            var idd = parseInt(Math.random() * 1000, 10);
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
                var idd = parseInt(Math.random() * 1000, 10);
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
                fiveArrIndexArr[i] = parseInt(Math.random() * 1000, 10);
                for (var j = 0; j < i; j++) {
                    if (fiveArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
            }
            // 生成4星数据
            for (var i = 0; i < 80; i++) {
                fourArrIndexArr[i] = parseInt(Math.random() * 1000, 10);
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
                fiveArrIndexArr[i] = parseInt(Math.random() * 1000, 10);
                for (var j = 0; j < i; j++) {
                    if (fiveArrIndexArr[i] === fiveArrIndexArr[j]) {
                        i--;
                        break;
                    }
                }
            }
            // 生成4星数据
            for (var i = 0; i < 60; i++) {
                fourArrIndexArr[i] = parseInt(Math.random() * 1000, 10);
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
        allproba() {
            let member = parseFloat(this.fiveProbxx) + parseFloat(this.fourProbxx);
            member = !!member ? member.toFixed(2) : 0;
            return member;
        },
        // 自动抽取每次10抽
        automatic(type) {
            this.clickColum++;
            if (this.clickColum <= 1) {
                let momIndx = 0;
                // 一个临时的数组用来储存1秒产生的数据
                let momArr = [];
                var arr = this.temArr;
                var mi = 0;
                this.time = setInterval(() => {
                    mi++;
                    momIndx++;
                    if (momIndx > 2) {
                        this.$refs.myEchars.addNewData(momArr, this.allproba());
                        momIndx = 1;
                        momArr = [];
                    }
                    for (var i = 0; i < 20; i++) {
                        if (mi > 10000) {
                            clearInterval(this.time);
                            return;
                        }
                        var idd = parseInt(Math.random() * 1000, 10);
                        this.getArr.push({
                            name: arr[idd].name,
                            type: arr[idd].type,
                            id: arr[idd].id
                        });
                        momArr.push({
                            name: arr[idd].name,
                            type: arr[idd].type,
                            id: arr[idd].id
                        });
                    }
                    if (this.getArr.length >= 4000) {
                        this.clearClick();
                    }
                }, 100);
            }
        },
        stop() {
            this.clickColum = 0;
            clearInterval(this.time);
        },
        // 清除
        clearClick() {
            this.getArr = [];
            this.$refs.myEchars.clear();
            this.clickColum = 0;
        },
        momAllrPro(data) {
            if (Number.parseFloat(data) >= 30) {
                this.hightProbabilities.push(data);
            }
        }
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
            return !!r ? r.toFixed(2) : 0;
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
            return !!r ? r.toFixed(2) : 0;
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.left-box {
    width: 34%;
    overflow: hidden;
    width: 60vw;
    border: solid 1px;
}
.btn-list{
    overflow: hidden;
    line-height: 30px;
    padding: 10px 0 0 0;
}
.btn-list button{
    margin-left: 5px;
}
.contant {
    display: flex;
    justify-content: space-between;
}
.right-box {
    width: 40vw;
}

.result-box{
    height: 50vh;
}
.result-item{
    text-align: left;
    height: 50vh;
    overflow-y: auto;
}
.result-title {
    font-size: 18px;
    font-weight: 700;
    padding: 5px;
}
.result-item span{
    display: inline-block;
    width: 13vw;
    text-align: center;
    margin-bottom: 10px;
}
.four{
    color: #0067ed;
}
.five{
    color: red;
}
.hello .echars-box, .hello .echars-box>div, .hello .canvas {
    width: 100vw;
    height: 30vh;
    overflow: hidden;
}
.show-hightProbability {
    border: solid 1px black;
    color: #6a09ff;
    min-height: 20px;
}
</style>
