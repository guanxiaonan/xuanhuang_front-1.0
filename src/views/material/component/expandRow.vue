<style lang="less">
    .advanced-router{height:42px;}
</style>

<template>
    <div>
        <Row class="advanced-router">
            <Col span="8">
                <span class="expand-key">创建时间：</span>
                <span class="expand-value">{{row.createTime | formatDate}}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">更新时间：</span>
                <span class="expand-value">{{ row.updateTime | formatDate }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">更新人：</span>
                <span class="expand-value">{{ row.modifierName }}</span>
            </Col>
        </Row>
    </div>
</template>
<script>
    import {formatDate} from '../../common/formatDate.js';
    export default {
        filters: {
            formatDate(time) {
                var date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm:ss');
            }
        },
        props: {
            row: Object
        }
    }
</script>