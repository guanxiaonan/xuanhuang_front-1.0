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
                    物料列表
                </p>
                <Table :columns="columns10" :data="data9"></Table>
            </Card>
        </Row>
    </div>
</template>

<script>
    import expandRow from './component/expandRow.vue';
    import axios from 'axios';
    export default {
        name: 'material-list',
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
                        title: '名称',
                        key: 'name'
                    },
                    {
                        title: '编码',
                        key: 'code'
                    },
                    {
                        title: '类型',
                        key: 'typeName'
                    },
                    {
                        title: '创建人',
                        key: 'creatorName'
                    }
                ],
                data9: []
            };
        },
        methods: {
            init () {
            },
            getData () {
                //请求方法，根据实际情况使用
                axios.get('/api/material').then((res) => {
                    //res 为成功回调的响应
                    this.data9 = res.data.data;
                });
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
