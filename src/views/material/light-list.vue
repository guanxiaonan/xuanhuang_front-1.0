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
                    光照数据
                </p>
                <Table :columns="columns10" :data="dataLight"></Table>
            </Card>
        </Row>
    </div>
</template>

<script>
    import expandRow from './component/expandRow.vue';
    import axios from 'axios';
    import {formatDate} from '../common/formatDate';
    export default {
        name: 'light-list',
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
                        title: '光照强度',
                        key: 'lux'
                    },
                    {
                        title: '日期',
                        key: 'date'
                    }
                ],
                dataLight: []
            };
        },
        methods: {
            init () {
            },

            testGit(){},
            // getData () {
            //     //请求方法，根据实际情况使用
            //     axios.get('/api/material').then((res) => {
            //         //res 为成功回调的响应
            //         this.data9 = res.data.data;
            //     });
            // },
            getData () {
                axios.get('/api/light_get').then((res) => {
                    for (var i = 0; i < res.data.data.length; i++) {
                        var date = new Date(res.data.data[i].date);
                        res.data.data[i].date = formatDate(date, 'yyyy-MM-dd hh:mm:ss');
                    }
                    this.dataLight = res.data.data;
                    console.log(res.data.data);
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
