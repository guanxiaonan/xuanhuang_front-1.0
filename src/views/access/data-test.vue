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
                console.log('开始生成随机数据')
                $.get('data/asset/data/life-expectancy-table.json', function (data) {
                    var symbolSize = 2.5;
                // 把配置和数据放这里
                this.chart.setOption({

                    grid3D: {},
                    xAxis3D: {
                        type: 'category'
                    },
                    yAxis3D: {},
                    zAxis3D: {},
                    dataset: {
                        dimensions: [
                            'Income',
                            'Life Expectancy',
                            'Population',
                            'Country',
                            {name: 'Year', type: 'ordinal'}
                        ],
                        source: data
                    },
                    series: [
                        {
                            type: 'scatter3D',
                            symbolSize: symbolSize,
                            encode: {
                                x: 'Country',
                                y: 'Life Expectancy',
                                z: 'Income',
                                tooltip: [0, 1, 2, 3, 4]
                            }
                        }
                    ]
                },

                    this.chart.setOption(option);

                })
            }
        }
    }
</script>
