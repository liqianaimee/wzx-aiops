<<<<<<< HEAD:src/components/page/Dashboard.vue
<template>
    <!-- <div>
        <el-row :gutter="20">
            <el-col :span="8">
                <el-card shadow="hover" class="mgb20" style="height:252px;">
                    <div class="user-info">
                        <img src="../../assets/img/img.jpg" class="user-avator" alt />
                        <div class="user-info-cont">
                            <div class="user-info-name">{{name}}</div>
                            <div>{{role}}</div>
                        </div>
                    </div>
                    <div class="user-info-list">
                        上次登录时间：
                        <span>2019-11-01</span>
                    </div>
                    <div class="user-info-list">
                        上次登录地点：
                        <span>东莞</span>
                    </div>
                </el-card>
                <el-card shadow="hover" style="height:252px;">
                    <div slot="header" class="clearfix">
                        <span>语言详情</span>
                    </div>Vue
                    <el-progress :percentage="71.3" color="#42b983"></el-progress>JavaScript
                    <el-progress :percentage="24.1" color="#f1e05a"></el-progress>CSS
                    <el-progress :percentage="13.7"></el-progress>HTML
                    <el-progress :percentage="5.9" color="#f56c6c"></el-progress>
                </el-card>
            </el-col>
            <el-col :span="16">
                <el-row :gutter="20" class="mgb20">
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{padding: '0px'}">
                            <div class="grid-content grid-con-1">
                                <i class="el-icon-lx-people grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">1234</div>
                                    <div>用户访问量</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{padding: '0px'}">
                            <div class="grid-content grid-con-2">
                                <i class="el-icon-lx-notice grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">321</div>
                                    <div>系统消息</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{padding: '0px'}">
                            <div class="grid-content grid-con-3">
                                <i class="el-icon-lx-goods grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">5000</div>
                                    <div>数量</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-card shadow="hover" style="height:403px;">
                    <div slot="header" class="clearfix">
                        <span>待办事项</span>
                        <el-button style="float: right; padding: 3px 0" type="text">添加</el-button>
                    </div>
                    <el-table :show-header="false" :data="todoList" style="width:100%;">
                        <el-table-column width="40">
                            <template slot-scope="scope">
                                <el-checkbox v-model="scope.row.status"></el-checkbox>
                            </template>
                        </el-table-column>
                        <el-table-column>
                            <template slot-scope="scope">
                                <div
                                    class="todo-item"
                                    :class="{'todo-item-del': scope.row.status}"
                                >{{scope.row.title}}</div>
                            </template>
                        </el-table-column>
                        <el-table-column width="60">
                            <template>
                                <i class="el-icon-edit"></i>
                                <i class="el-icon-delete"></i>
                            </template>
                        </el-table-column>
                    </el-table>
                </el-card>
            </el-col>
        </el-row>
        <el-row :gutter="20">
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="bar" class="schart" canvasId="bar" :options="options"></schart>
                </el-card>
            </el-col>
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="line" class="schart" canvasId="line" :options="options2"></schart>
                </el-card>
            </el-col>
        </el-row>
    </div> -->
    <div id='content'>

        <div style="display:flex">

            <div id="main2" style="width: 50vw;height:400px;"></div>
            <div id="main3" style="width: 50vw;height:400px;"></div>
        </div>
        <div style="display:flex">
            <div id="main1" style="width: 400px;height:400px;"></div>
            <div id="main4" style="width: 800px;height:400px;"></div>
            <h2 style="color:grey">在线人数{{onlineData}}人
                {{this.currentTime}}
            </h2>
        </div>


    </div>

</template>

<script>
    // import Schart from 'vue-schart';
    // import bus from '../common/bus';
    // 引入 ECharts 主模块
    import * as echarts from 'echarts'
    import theme from '../../utils/walden.json'
    import 'echarts-gl'
    export default {

        data() {
            return {
                onlineData: 0,
                currentTime: null
            }

        },
        async created() {

        },
        methods: {
            async drawChart() {
                const {
                    data: res1
                } = await this.$http.get('http://146.56.200.122:8080/actuator/metrics/system.cpu.usage')
                let cpu = (res1.measurements[0].value).toFixed(2)
                console.log(cpu)

                const {
                    data: res2
                } = await this.$http.get('http://146.56.200.122:8080/getMemory')
                let edenList = res2.content.edenList
                let oldList = res2.content.oldList

                const {
                    data: res3
                } = await this.$http.get('http://146.56.200.122:8080/httpCount')
                let httpCount = res3.content

                const {
                    data: res4
                } = await this.$http.get('http://146.56.200.122:8080/selectEvent')
                let list = res4.content

                const {
                    data: res5
                } = await this.$http.get(
                    'http://146.56.200.122:8080/actuator/metrics/tomcat.sessions.active.current')
                this.onlineData = res5.measurements[0].value

                let obj = theme
                echarts.registerTheme('purple-passsion', obj)
                var chartDom1 = document.getElementById('main1');
                var chartDom2 = document.getElementById('main2');
                var chartDom3 = document.getElementById('main3');
                var chartDom4 = document.getElementById('main4');


                var myChart1 = echarts.init(chartDom1, 'purple-passsion');
                var myChart2 = echarts.init(chartDom2, 'purple-passsion');
                var myChart3 = echarts.init(chartDom3, 'purple-passsion');
                var myChart4 = echarts.init(chartDom4, 'purple-passsion');
                var option1, option2, option3, option4, xaxisData = [];

                let H, M, tempTime
                tempTime = new Date()
                H = tempTime.getHours()
                M = tempTime.getMinutes()
                xaxisData.push(H + ':' + M)
                for (let i = 0; i < 9; i++) {
                    let tempH, tempM, hStr, mStr
                    tempTime = new Date(tempTime.getTime() - 60000)
                    tempH = tempTime.getHours()
                    tempM = tempTime.getMinutes()
                    //补零
                    hStr = tempH >= 10 ? tempH : `0${tempH}`
                    mStr = tempM >= 10 ? tempM : `0${tempM}`
                    xaxisData.push(hStr + ':' + mStr)
                }
                xaxisData.reverse()

                option1 = {
                    title: {
                        text: 'cpu使用情况'
                    },
                    tooltip: {
                        trigger: 'item',

                    },
                    color: ['#3fb1e3', '#ddd'],

                    series: [{
                        // name: 'cpu使用情况',
                        type: 'pie',
                        //控制圆环的大小
                        radius: ['60%', '70%'],
                        hoverAnimation: false, //鼠标悬浮是否有区域弹出动画
                        // avoidLabelOverlap: false,
                        //不显示名称
                        label: {
                            formatter: `cpu占用比\n${cpu*100}%`,
                            show: true,
                            position: 'center',
                            fontSize: '30',
                            fontWeight: 'bold'
                        },
                        // //当鼠标掠过
                        // emphasis: {
                        //     //在中间显示名称
                        //     label: {
                        //         show: true,

                        //     }
                        // },
                        labelLine: {
                            //隐藏指示线
                            show: false
                        },
                        data: [{
                                value: cpu * 100,
                                name: `cpu占用率${cpu*100}%`
                            },
                            {
                                value: (1 - cpu) * 100,
                                // name: '剩余cpu'
                            }
                        ]

                    }]
                };
                option1 && myChart1.setOption(option1);
                option2 = {
                    title: {
                        text: '内存使用情况'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    xAxis: {
                        type: 'category',
                        data: xaxisData,
                        boundaryGap: false //和y轴没有间隙
                    },
                    yAxis: {
                        type: 'value'
                    },
                    // 可选
                    legend: {
                        top: '5%',
                        left: 'center'
                    },

                    series: [{
                            data: edenList,
                            type: 'line',
                            name: '新生代内存(GB)'
                        },
                        {
                            data: oldList,
                            type: 'line',
                            name: '老生代内存(GB)'
                        }
                    ]
                };
                option2 && myChart2.setOption(option2);

                option3 = {
                    title: {
                        text: '每分钟请求次数'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    xAxis: {
                        type: 'category',
                        data: xaxisData,
                        boundaryGap: false //和y轴没有间隙
                    },
                    yAxis: {
                        type: 'value'
                    },
                    // 可选
                    legend: {
                        top: '5%',
                        left: 'center'
                    },

                    series: [{
                        data: httpCount,
                        type: 'line',
                        name: '请求次数'
                    }]
                };
                option3 && myChart3.setOption(option3);

                let cateData = [],
                    valueData = []
                for (let key in list) {
                    cateData.push(key)
                    // if (list[key] > 50) {
                    //     valueData.push({
                    //         value: list[key],
                    //         itemStyle: {
                    //             color: 'red'
                    //         }
                    //     })
                    // } else if (list[key] > 10) {
                    //     valueData.push({
                    //         value: list[key],
                    //         itemStyle: {
                    //             color: 'orange'
                    //         }
                    //     })
                    // } else {
                    valueData.push(list[key])


                    // }

                }
                option4 = {
                    title: {
                        text: '事件统计'
                    },
                    tooltip: {
                        trigger: 'axis',
                        axisPointer: 'shadow'
                    },
                    xAxis: {
                        type: 'category',
                        data: cateData
                    },
                    yAxis: {
                        type: 'value',

                    },
                    series: [{
                        // data: [120, {
                        //     value: 200,
                        //     itemStyle: {
                        //         color: '#a90000'
                        //     }
                        // }, 150, 80, 70, 110, 130],
                        data: valueData,
                        type: 'bar',
                        itemStyle: {
                            normal: {
                                color: function (params) {
                                    let colorList = [
                                        '#3fb1e3',
                                        'orange',
                                        'red'
                                    ]
                                    return colorList[params.dataIndex]
                                }
                            }
                            // color: {
                            //     type: 'linear',
                            //     x: 0,
                            //     y: 0,
                            //     x2: 0,
                            //     y2: 1,
                            //     colorStops: [{
                            //             offset: 0,
                            //             color: 'rgba(20,200,212,0.5)'
                            //         },
                            //         {
                            //             offset: 0.2,
                            //             color: 'rgba(20,200,212,0.2)'
                            //         },
                            //         {
                            //             offset: 1,
                            //             color: 'rgba(20,200,212,0)'
                            //         }
                            //     ]

                            // }

                        }
                    }]
                };

                option4 && myChart4.setOption(option4);
                // var xData = ['info', 'warn', 'error'];
                // var days = [''];

                // var data = [
                //     [0, 0],
                //     [0, 1],
                //     [0, 2]
                // ];
                // data[0].push(list['info'])
                // data[1].push(list['warn'])
                // data[2].push(list['error'])
                // option4 = {
                //     tooltip: {},
                //     visualMap: {
                //         show: false,
                //         max: 15,
                //         inRange: {
                //             color: ['#3fb1e3', '#4575b4', '#74add1', '#abd9e9', '#e0f3f8', '#ffffbf', '#fee090',
                //                 '#fdae61', '#f46d43', '#d73027', '#a50026'
                //             ]
                //         }
                //     },
                //     xAxis3D: {
                //         name: 'x',
                //         nameGap: 1,
                //         type: 'category',
                //         data: xData
                //     },
                //     yAxis3D: {
                //         name: '',
                //         type: 'category',
                //         data: days
                //     },
                //     zAxis3D: {
                //         name: '',
                //         type: 'value'
                //     },
                //     grid3D: {
                //         boxWidth: 150,
                //         boxDepth: 20,
                //         light: {
                //             main: {
                //                 intensity: 1.2,
                //                 shadow: true
                //             },
                //             ambient: {
                //                 intensity: 0.2
                //             }
                //         },
                //         viewControl: {
                //             alpha: 10,
                //             beta: 10,
                //         }
                //     },
                //     series: [{
                //         type: 'bar3D',
                //         data: data.map(function (item) {
                //             return {
                //                 value: [item[1], item[0], item[2]],
                //             }
                //         }),
                //         shading: 'lambert',
                //         label: {
                //             textStyle: {
                //                 fontSize: 16,
                //                 borderWidth: 1
                //             }
                //         },

                //         emphasis: {
                //             label: {
                //                 textStyle: {
                //                     fontSize: 20,
                //                 }
                //             },
                //         }
                //     }]
                // }
                // option4 && myChart4.setOption(option4);


            }
        },
        mounted() {
            this.drawChart(this.cpu)
        }
        // name: 'dashboard',
        // data() {
        //     return {
        //         name: localStorage.getItem('ms_username'),
        //         todoList: [
        //             {
        //                 title: '今天要修复100个bug',
        //                 status: false
        //             },
        //             {
        //                 title: '今天要修复100个bug',
        //                 status: false
        //             },
        //             {
        //                 title: '今天要写100行代码加几个bug吧',
        //                 status: false
        //             },
        //             {
        //                 title: '今天要修复100个bug',
        //                 status: false
        //             },
        //             {
        //                 title: '今天要修复100个bug',
        //                 status: true
        //             },
        //             {
        //                 title: '今天要写100行代码加几个bug吧',
        //                 status: true
        //             }
        //         ],
        //         data: [
        //             {
        //                 name: '2018/09/04',
        //                 value: 1083
        //             },
        //             {
        //                 name: '2018/09/05',
        //                 value: 941
        //             },
        //             {
        //                 name: '2018/09/06',
        //                 value: 1139
        //             },
        //             {
        //                 name: '2018/09/07',
        //                 value: 816
        //             },
        //             {
        //                 name: '2018/09/08',
        //                 value: 327
        //             },
        //             {
        //                 name: '2018/09/09',
        //                 value: 228
        //             },
        //             {
        //                 name: '2018/09/10',
        //                 value: 1065
        //             }
        //         ],
        //         options: {
        //             type: 'bar',
        //             title: {
        //                 text: '最近一周各品类销售图'
        //             },
        //             xRorate: 25,
        //             labels: ['周一', '周二', '周三', '周四', '周五'],
        //             datasets: [
        //                 {
        //                     label: '家电',
        //                     data: [234, 278, 270, 190, 230]
        //                 },
        //                 {
        //                     label: '百货',
        //                     data: [164, 178, 190, 135, 160]
        //                 },
        //                 {
        //                     label: '食品',
        //                     data: [144, 198, 150, 235, 120]
        //                 }
        //             ]
        //         },
        //         options2: {
        //             type: 'line',
        //             title: {
        //                 text: '最近几个月各品类销售趋势图'
        //             },
        //             labels: ['6月', '7月', '8月', '9月', '10月'],
        //             datasets: [
        //                 {
        //                     label: '家电',
        //                     data: [234, 278, 270, 190, 230]
        //                 },
        //                 {
        //                     label: '百货',
        //                     data: [164, 178, 150, 135, 160]
        //                 },
        //                 {
        //                     label: '食品',
        //                     data: [74, 118, 200, 235, 90]
        //                 }
        //             ]
        //         }
        //     };
        // },
        // components: {
        //     Schart
        // },
        // computed: {
        //     role() {
        //         return this.name === 'admin' ? '超级管理员' : '普通用户';
        //     }
        // },
        // // created() {
        // //     this.handleListener();
        // //     this.changeDate();
        // // },
        // // activated() {
        // //     this.handleListener();
        // // },
        // // deactivated() {
        // //     window.removeEventListener('resize', this.renderChart);
        // //     bus.$off('collapse', this.handleBus);
        // // },
        // methods: {
        //     changeDate() {
        //         const now = new Date().getTime();
        //         this.data.forEach((item, index) => {
        //             const date = new Date(now - (6 - index) * 86400000);
        //             item.name = `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`;
        //         });
        //     }
        //     // handleListener() {
        //     //     bus.$on('collapse', this.handleBus);
        //     //     // 调用renderChart方法对图表进行重新渲染
        //     //     window.addEventListener('resize', this.renderChart);
        //     // },
        //     // handleBus(msg) {
        //     //     setTimeout(() => {
        //     //         this.renderChart();
        //     //     }, 200);
        //     // },
        //     // renderChart() {
        //     //     this.$refs.bar.renderChart();
        //     //     this.$refs.line.renderChart();
        //     // }
        // }
    };
</script>


<style scoped>
    .el-row {
        margin-bottom: 20px;
    }

    .grid-content {
        display: flex;
        align-items: center;
        height: 100px;
    }

    .grid-cont-right {
        flex: 1;
        text-align: center;
        font-size: 14px;
        color: #999;
    }

    .grid-num {
        font-size: 30px;
        font-weight: bold;
    }

    .grid-con-icon {
        font-size: 50px;
        width: 100px;
        height: 100px;
        text-align: center;
        line-height: 100px;
        color: #fff;
    }

    .grid-con-1 .grid-con-icon {
        background: rgb(45, 140, 240);
    }

    .grid-con-1 .grid-num {
        color: rgb(45, 240, 230);
    }

    .grid-con-2 .grid-con-icon {
        background: rgb(100, 213, 114);
    }

    .grid-con-2 .grid-num {
        color: rgb(45, 140, 240);
    }

    .grid-con-3 .grid-con-icon {
        background: rgb(242, 94, 67);
    }

    .grid-con-3 .grid-num {
        color: rgb(242, 94, 67);
    }

    .user-info {
        display: flex;
        align-items: center;
        padding-bottom: 20px;
        border-bottom: 2px solid #ccc;
        margin-bottom: 20px;
    }

    .user-avator {
        width: 120px;
        height: 120px;
        border-radius: 50%;
    }

    .user-info-cont {
        padding-left: 50px;
        flex: 1;
        font-size: 14px;
        color: #999;
    }

    .user-info-cont div:first-child {
        font-size: 30px;
        color: #222;
    }

    .user-info-list {
        font-size: 14px;
        color: #999;
        line-height: 25px;
    }

    .user-info-list span {
        margin-left: 70px;
    }

    .mgb20 {
        margin-bottom: 20px;
    }

    .todo-item {
        font-size: 14px;
    }

    .todo-item-del {
        text-decoration: line-through;
        color: #999;
    }

    .schart {
        width: 100%;
        height: 300px;
    }
</style>
=======
<template>
    <div>
        <el-row :gutter="20">
            <el-col :span="8">
                <el-card shadow="hover" class="mgb20" style="height:252px;">
                    <div class="user-info">
                        <img src="../assets/img/img.jpg" class="user-avator" alt />
                        <div class="user-info-cont">
                            <div class="user-info-name">{{ name }}</div>
                            <div>{{ role }}</div>
                        </div>
                    </div>
                    <div class="user-info-list">
                        上次登录时间：
                        <span>2019-11-01</span>
                    </div>
                    <div class="user-info-list">
                        上次登录地点：
                        <span>东莞</span>
                    </div>
                </el-card>
                <el-card shadow="hover" style="height:252px;">
                    <template #header>
                        <div class="clearfix">
                            <span>语言详情</span>
                        </div>
                    </template>
                    Vue
                    <el-progress :percentage="71.3" color="#42b983"></el-progress>JavaScript
                    <el-progress :percentage="24.1" color="#f1e05a"></el-progress>CSS
                    <el-progress :percentage="13.7"></el-progress>HTML
                    <el-progress :percentage="5.9" color="#f56c6c"></el-progress>
                </el-card>
            </el-col>
            <el-col :span="16">
                <el-row :gutter="20" class="mgb20">
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{ padding: '0px' }">
                            <div class="grid-content grid-con-1">
                                <i class="el-icon-user-solid grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">1234</div>
                                    <div>用户访问量</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{ padding: '0px' }">
                            <div class="grid-content grid-con-2">
                                <i class="el-icon-message-solid grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">321</div>
                                    <div>系统消息</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                    <el-col :span="8">
                        <el-card shadow="hover" :body-style="{ padding: '0px' }">
                            <div class="grid-content grid-con-3">
                                <i class="el-icon-s-goods grid-con-icon"></i>
                                <div class="grid-cont-right">
                                    <div class="grid-num">5000</div>
                                    <div>数量</div>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-card shadow="hover" style="height:403px;">
                    <template #header>
                        <div class="clearfix">
                            <span>待办事项</span>
                            <el-button style="float: right; padding: 3px 0" type="text">添加</el-button>
                        </div>
                    </template>

                    <el-table :show-header="false" :data="todoList" style="width:100%;">
                        <el-table-column width="40">
                            <template #default="scope">
                                <el-checkbox v-model="scope.row.status"></el-checkbox>
                            </template>
                        </el-table-column>
                        <el-table-column>
                            <template #default="scope">
                                <div
                                    class="todo-item"
                                    :class="{
                                        'todo-item-del': scope.row.status,
                                    }"
                                >{{ scope.row.title }}</div>
                            </template>
                        </el-table-column>
                        <el-table-column width="60">
                            <template>
                                <i class="el-icon-edit"></i>
                                <i class="el-icon-delete"></i>
                            </template>
                        </el-table-column>
                    </el-table>
                </el-card>
            </el-col>
        </el-row>
        <el-row :gutter="20">
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="bar" class="schart" canvasId="bar" :options="options"></schart>
                </el-card>
            </el-col>
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="line" class="schart" canvasId="line" :options="options2"></schart>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import Schart from "vue-schart";
export default {
    name: "dashboard",
    data() {
        return {
            name: localStorage.getItem("ms_username"),
            todoList: [
                {
                    title: "今天要修复100个bug",
                    status: false
                },
                {
                    title: "今天要修复100个bug",
                    status: false
                },
                {
                    title: "今天要写100行代码加几个bug吧",
                    status: false
                },
                {
                    title: "今天要修复100个bug",
                    status: false
                },
                {
                    title: "今天要修复100个bug",
                    status: true
                },
                {
                    title: "今天要写100行代码加几个bug吧",
                    status: true
                }
            ],
            data: [
                {
                    name: "2018/09/04",
                    value: 1083
                },
                {
                    name: "2018/09/05",
                    value: 941
                },
                {
                    name: "2018/09/06",
                    value: 1139
                },
                {
                    name: "2018/09/07",
                    value: 816
                },
                {
                    name: "2018/09/08",
                    value: 327
                },
                {
                    name: "2018/09/09",
                    value: 228
                },
                {
                    name: "2018/09/10",
                    value: 1065
                }
            ],
            options: {
                type: "bar",
                title: {
                    text: "最近一周各品类销售图"
                },
                xRorate: 25,
                labels: ["周一", "周二", "周三", "周四", "周五"],
                datasets: [
                    {
                        label: "家电",
                        data: [234, 278, 270, 190, 230]
                    },
                    {
                        label: "百货",
                        data: [164, 178, 190, 135, 160]
                    },
                    {
                        label: "食品",
                        data: [144, 198, 150, 235, 120]
                    }
                ]
            },
            options2: {
                type: "line",
                title: {
                    text: "最近几个月各品类销售趋势图"
                },
                labels: ["6月", "7月", "8月", "9月", "10月"],
                datasets: [
                    {
                        label: "家电",
                        data: [234, 278, 270, 190, 230]
                    },
                    {
                        label: "百货",
                        data: [164, 178, 150, 135, 160]
                    },
                    {
                        label: "食品",
                        data: [74, 118, 200, 235, 90]
                    }
                ]
            }
        };
    },
    components: {
        Schart
    },
    computed: {
        role() {
            return this.name === "admin" ? "超级管理员" : "普通用户";
        }
    },

    methods: {
        changeDate() {
            const now = new Date().getTime();
            this.data.forEach((item, index) => {
                const date = new Date(now - (6 - index) * 86400000);
                item.name = `${date.getFullYear()}/${date.getMonth() +
                    1}/${date.getDate()}`;
            });
        }
    }
};
</script>

<style scoped>
.el-row {
    margin-bottom: 20px;
}

.grid-content {
    display: flex;
    align-items: center;
    height: 100px;
}

.grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
}

.grid-num {
    font-size: 30px;
    font-weight: bold;
}

.grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
}

.grid-con-1 .grid-con-icon {
    background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
    background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
    background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
    color: rgb(242, 94, 67);
}

.user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
}

.user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
}

.user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
}

.user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
}

.user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
}

.user-info-list span {
    margin-left: 70px;
}

.mgb20 {
    margin-bottom: 20px;
}

.todo-item {
    font-size: 14px;
}

.todo-item-del {
    text-decoration: line-through;
    color: #999;
}

.schart {
    width: 100%;
    height: 300px;
}
</style>
>>>>>>> 221aa3a5b9de7356f3f08e147bc98515a2880cad:src/views/Dashboard.vue
