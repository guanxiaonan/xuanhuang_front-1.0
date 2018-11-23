<template>
    <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart">
    </div>
</template>
<script>
    import echarts from 'echarts';
    import axios from 'axios'
    export default {
        props: {
            className: {
                type: String,
                default: 'yourClassName'
            },
            id: {
                type: String,
                default: 'yourID'
            },
            width: {
                type: String,
                default: '1000px'
            },
            height: {
                type: String,
                default: '500px'
            }
        },
        data() {
            return {
                chart: null
            }
        },
        mounted() {
            this.initChart();
        },
        beforeDestroy() {
            if (!this.chart) {
                return
            }
            this.chart.dispose();
            this.chart = null;
        },
        methods: {
            getData () {
                axios.get('/api/light_get').then((res) => {
                    for (var i = 0; i < res.data.data.length; i++) {
                        var date = new Date(res.data.data[i].date);
                        res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
                    }
                    this.dataLight = res.data.data;

                }).catch((error) => {
                    console.log('light数据接受完成');
                });
            },

            // getData () {
            //     axios.get('/api/light_get').then((res) => {
            //         for (var i = 0; i < res.data.data.length; i++) {
            //             var date = new Date(res.data.data[i].date);
            //             res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
            //         }
            //         this.dataLight = res.data.data;
            //         console.log(res.data.data);
            //     }).catch((error) => {
            //         console.log(error);
            //     });
            // },

            initChart() {
                this.chart = echarts.init(this.$refs.myEchart);
                // 把配置和数据放这里
                this.chart.setOption({
                    title: {
                        text: '轩黄黄茶'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                        data: ['周一','周二','周三','周四','周五','周六','周日','周二','周三','周四','周五','周六','周日','周二','周三','周四','周五','周六','周日','周二','周三','周四','周五','周六','周日']
                    },
                    yAxis: {
                        splitLine: {
                            show: false
                        }
                    },
                    toolbox: {
                        left: 'center',
                        feature: {
                            dataZoom: {
                                yAxisIndex: 'none'
                            },
                            restore: {},
                            saveAsImage: {}
                        }
                    },
                    dataZoom: [{
                        startValue: '2014-06-01'
                    }, {
                        type: 'inside'
                    }],
                    visualMap: {
                        top: 10,
                        right: 10,
                        pieces: [{
                            gt: 0,
                            lte: 50,
                            color: '#096'
                        }, {
                            gt: 50,
                            lte: 100,
                            color: '#ffde33'
                        }, {
                            gt: 100,
                            lte: 150,
                            color: '#ff9933'
                        }, {
                            gt: 150,
                            lte: 200,
                            color: '#cc0033'
                        }, {
                            gt: 200,
                            lte: 300,
                            color: '#660099'
                        }, {
                            gt: 300,
                            color: '#7e0023'
                        }],
                        outOfRange: {
                            color: '#999'
                        }
                    },
                    series: [
                        {
                            name: '邮件营销',
                            type: 'line',
                            stack: '总量',
                            data:[120, 132, 101, 134, 90, 230, 210,32, 101, 134, 90, 230, 210, 90, 230, 210,32, 101, 134, 90, 230, 210, 90, 230, 210,32, 101, 134, 90, 230, 210]
                        },
                        {
                            name: '联盟广告',
                            type: 'line',
                            stack: '总量',
                            data:[220, 182, 191, 234, 290, 330, 310, 182, 191, 234, 290, 330, 31, 182, 191, 234, 290, 330, 31, 182, 191, 234, 290, 330, 31]
                        },
                        {
                            name: '视频广告',
                            type: 'line',
                            stack: '总量',
                            data:[150, 232, 201, 154, 190, 330, 410, 201, 154, 190, 330, 410, 201, 154, 190, 330, 410, 201, 154, 190, 330, 410, 201, 154, 190, 330, 410]
                        },
                        {
                            name: '直接访问',
                            type: 'line',
                            stack: '总量',
                            data:[320, 332, 301, 334, 390, 330, 320, 332, 301, 334, 390, 330, 320, 332, 301, 334, 390, 330, 320, 332, 301, 334, 390, 330, 320, 332, 301, 334, 390, 330, 320]
                        },
                        {
                            name: '搜索引擎',
                            type: 'line',
                            stack: '总量',
                            data:[820, 932, 901, 934, 1290, 1330, 1320, 901, 934, 1290, 1330, 1320, 901, 934, 1290, 1330, 1320, 901, 934, 1290, 1330, 1320, 901, 934, 1290, 1330, 1320, 901, 934, 1290, 1330, 1320]
                        }
                    ]
                })
            }
        }
    }
</script>
