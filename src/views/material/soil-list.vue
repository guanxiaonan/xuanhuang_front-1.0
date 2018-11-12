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
        </Row>
    </div>
</template>

<script>
    import expandRow from './component/expandRow.vue';
    import axios from 'axios';
    import {formatDate} from '../common/formatDate'
    export default {
        name: 'soil-list',
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
                    console.log(res.data.data);
                    for (var i = 0; i < res.data.data.length; i++) {
                        var date = new Date(res.data.data[i].date);
                        res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
                    }
                    this.dataSoil = res.data.data;
                }).catch((error) => {
                    console.log(error);
                });
            }
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
