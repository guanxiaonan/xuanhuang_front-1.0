<style lang="less">
    @import '../../styles/common.less';
    .advanced-router{
        &-tip-p{
            padding: 10px 0;
        }
    }
</style>

<template>
    <div>
        <Row>
            <Card>
                <p slot="title">
                    <Icon type="compose"></Icon>
                    空气数据
                </p>
                <Table :columns="columns10" :data="dataAir"></Table>
            </Card>
            <card>
                <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart">
                </div>
            </card>
        </Row>
    </div>
</template>

<script>
    import expandRow from './component/expandRow.vue';
    import axios from 'axios';
    import {formatDate} from '../common/formatDate';
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
        name: 'air-list',
        components: {
            expandRow
        },
        data () {
            return {
                columns10: [
                    {
                        type: 'expand',
                        width: 50,
                        render: (h, params) => {
                            return h(expandRow, {
                                props: {
                                    row: params.row
                                }
                            });
                        }
                    },
                    {
                        title: '茶园编号',
                        key: 'teagarId'
                    },
                    {
                        title: '茶园名',
                        key: 'teagarName'
                    },
                    {
                        title: '空气温度',
                        key: 'airTemperature'
                    },
                    {
                        title: '空气湿度',
                        key: 'airHumidity'
                    },
                    {
                        title: '日期',
                        key: 'date'
                    }
                ],
                dataAir: [],
                date: [],
                airHumidity:[],
                airTemperature:[],
            };
        },
        methods: {
            init () {
            },
            // getData () {
            //     //请求方法，根据实际情况使用
            //     axios.get('/api/material').then((res) => {
            //         //res 为成功回调的响应
            //         this.data9 = res.data.data;
            //     });
            // },
            // formatDate (time) {
            //     var date = new Date(time);
            //     return formatDate(date, 'yyyy-MM-dd hh:mm:ss');
            // },

            getData () {
                axios.get('api/air_get').then((res) => {
                    for (var i = 0; i < res.data.data.length; i++) {
                        var date = new Date(res.data.data[i].date);
                        // res.data.data[i].data = this.formatDate(res.data.data[i].data);
                        res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
                        this.data=res.data.data.data;
                        this.airHumidity[i]=res.data.data[i].airHumidity;
                        this.date[i]=res.data.data[i].date;
                        this.airTemperature[i]=res.data.data[i].airTemperature;
                    }
                    this.dataAir = res.data.data;
                    console.log(this.airHumidity[0]);
                    console.log(this.date[0]);
                    console.log(res.data.data);
                    this.initChart();
                }).catch((error) => {
                    console.log(error);
                });

            },
            initChart() {
                console.log('开始画图');
                this.chart = echarts.init(this.$refs.myEchart);
                // console.log(this.date);
                // console.log(10086);
                // 把配置和数据放这里
                this.chart.setOption({
                        xAxis: {
                            type: 'category',
                            data: this.date,
                            // data: [1,2,3,4,5,67]
                        },
                        yAxis: {
                            type: 'value',
                        },
                        series: [{
                            data: this.airHumidity,
                            type: 'line'
                        },{
                            data:this.airTemperature,
                            type: 'line'
                        }
                            ]
                    },
                )
                console.log('画图结束');
            },
        },
        mounted () {
            this.init();
            this.getData();
        },
        watch: {
            '$route' () {
                this.init();
            }
        }
    };
</script>
