<template>
    <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart">
    </div>
</template>
<script>
    import echarts from 'echarts'
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
            initChart() {
                this.chart = echarts.init(this.$refs.myEchart);
                // 把配置和数据放这里
                this.chart.setOption({
                    title: {
                        text: '元素数据变化图'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    legend: {
                        data:['A','B','C','D','E']
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                        data: ['周一','周二','周三','周四','周五','周六','周日']
                    },
                    yAxis: {
                        type: 'value'
                    },
                    series: [
                        {
                            name:'A',
                            type:'line',
                            stack: '总量',
                            data:[120, 132, 101, 134, 90, 230, 210]
                        },
                        {
                            name:'B',
                            type:'line',
                            stack: '总量',
                            data:[220, 182, 191, 234, 290, 330, 310]
                        },
                        {
                            name:'C',
                            type:'line',
                            stack: '总量',
                            data:[150, 232, 201, 154, 190, 330, 410]
                        },
                        {
                            name:'D',
                            type:'line',
                            stack: '总量',
                            data:[320, 332, 301, 334, 390, 330, 320]
                        },
                        {
                            name:'E',
                            type:'line',
                            stack: '总量',
                            data:[820, 932, 901, 934, 1290, 1330, 1320]
                        }
                    ]
                })
            }
        }
    }
</script>
