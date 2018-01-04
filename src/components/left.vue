<template>
    <div class="left">
        <ul>
            <li @click="download">
                <Tooltip title="提示标题" content="提示内容" placement="right">
                    <Icon type="ios-cloud-download" class="icon"></Icon>
                </Tooltip>
            </li>
            <li @click="squaredUp" title="33222">
                <Icon type="ios-information" class="icon"></Icon>
                <!-- <Icon type="qr-scanner" class="icon"></Icon> -->
            </li>
            <li @click="upload">
                <!-- <Icon type="ios-cloud-upload" class="icon"></Icon> -->
                <Icon type="help-circled" class="icon"></Icon>
            </li>
            <li></li>
        </ul>
    </div>
</template>

<script>
    import bus from '../bus'
    export default {
        data() {
            return {
            }
        },
        mounted() {},
        methods: {
            // 经纬度导出
            download() {
                // 发射
                // bus.$emit("love",0);
                bus.$emit("tabDisplay", 0)
            },
            squaredUp() {
                // bus.$emit("love",1);
                bus.$emit("tabDisplay", 1)
            },
            upload() {
                alert("第三个事件")
            },
        }
    }
</script>

<style scope>
    .left {
        position: fixed;
        top: 68px;
        width: 100px;
        height: 100%;
        background: #00CCCC
    }
    .left li i {
        margin-left: 30px;
        margin-top: 20px;
        cursor: pointer;
    }
    .icon {
        font-size: 42px;
        color: white;
    }
    .center-right {
        float: right;
    }
    li {
        z-index: 30
    }
</style>
