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
                    土壤数据
                </p>
                <Table :columns="columns10" :data="dataSoil"></Table>
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
        name: 'soil-list',
        components: {
            expandRow
        },
        data () {
            return {
                date: [],
                soilHumidity: [],
                chart: null,
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
                        title: '土壤温度',
                        key: 'soilTemperature'
                    },
                    {
                        title: '土壤湿度',
                        key: 'soilHumidity'
                    },
                    {
                        title: '日期',
                        key: 'date'
                    }
                ],
                dataSoil: []
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
            // timestampToTime(timestamp) {
            //     var date = new Date(timestamp);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
            //     var Y = date.getFullYear() + '-';
            //     var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
            //     var D = date.getDate() + ' ';
            //     var h = date.getHours() + ':';
            //     var m = date.getMinutes() + ':';
            //     var s = date.getSeconds();
            //     return Y + M + D + h + m + s;
            // },
            getData () {
                axios.get('api/soil_get').then((res) => {
                    // if(res.data.data.length === 0) {
                    //
                    // }
                    // console.log(res.data.data);
                    // console.log("==========", res.data.data[0])
                    // console.log(res.data.data[0].soilHumidity);
                    for (var i = 0; i < res.data.data.length; i++) {
                        var date = new Date(res.data.data[i].date);
                        res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
                        this.date[i]=res.data.data[i].date;
                        this.soilHumidity[i]=res.data.data[i].soilHumidity;
                    }

                    // console.log(this.soilHumidity[0])
                    // console.log(this.soilHumidity)
                    // console.log(this.date)
                    this.dataSoil = res.data.data;
                    this.initChart()
                }).catch((error) => {
                    console.log(error);
                });
            },
            initChart() {
                // console.log('开始画图');
                this.chart = echarts.init(this.$refs.myEchart);
                // console.log(this.date);
                // console.log(10086);
                // 把配置和数据放这里
                this.chart.setOption({
                    xAxis: {
                        type: 'category',
                        data: this.date
                        // data: [1,2,3,4,5,67]
                    },
                    yAxis: {
                        type: 'value',
                    },
                    series: [{
                        data: this.soilHumidity,
                        type: 'line'
                    }]
                },
                )
                console.log('画图结束');
            },
        },
        created(){
        },
        mounted(){
            this.getData();
            this.initChart();
            this.init();
        },
        beforeDestroy(){
            if (!this.chart) {
                return
            }
            this.chart.dispose();
            this.chart = null;
        },
        watch: {
            '$route'(){
                this.init();
            }
        }
    };
</script>
