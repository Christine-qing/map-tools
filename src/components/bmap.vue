<template>
    <div class="bigmap">
        <div id="allmap"> </div>
        <div class="btn">
            <!-- 依次是放大，缩小，打点，取消打点，清空 -->
            <Button-group vertical>
                <Button type="info" icon="plus-round" @click="enlarge"></Button>
                <Button type="info" icon="minus-round" @click="lessen"></Button>
                <Button type="info" icon="edit" @click="capture"></Button>
                <Button type="info" icon="android-hand" @click="hand"></Button>
                <Button type="info" icon="trash-a" @click="clearAll"></Button>
            </Button-group>
        </div>
    </div>
</template>

<script>
    import bus from '../bus'
    export default {
        data() {
            return {
                index: 0,
                map: null,
                overlays: [],
            }
        },
        mounted() {
            bus.$on("love", this.change)
            this.init()
        },
        methods: {
            // init初始化地图
            init(event) {
                let map = new BMap.Map('allmap', {
                    enableMapClick: true
                });
                map.centerAndZoom(new BMap.Point(116.404, 39.915), 11);
                map.addControl(new BMap.MapTypeControl()); //添加地图类型控件 卫星 地图 三维
                map.setCurrentCity("北京");
                map.enableScrollWheelZoom(true); //开启鼠标滚轮缩放      
                this.map = map;
            },
            change(i) {
                this.index = i;
            },
            // 打点
            capture(e) {
                this.map.addEventListener("click", this.captureOpen);
            },
            // 打点
            captureOpen(e) {
                let point = new BMap.Point(e.point.lng, e.point.lat)
                let marker = new BMap.Marker(point);
                this.map.addOverlay(marker);
                //当绘制一个覆盖物，push到空数组中
                this.overlays.push(marker);
                bus.$emit("tabVal", point)
            },
            // 切换手型
            hand() {
                this.map.removeEventListener("click", this.captureOpen, false);
            },
            // 清空地图
            clearAll() {
                for (var i = 0; i < this.overlays.length; i++) {
                    var item = this.overlays[i];
                    this.map.removeOverlay(item);
                }
                this.overlays.length = 0;
                this.map.removeEventListener("click", this.captureOpen, false);
                bus.$emit("tabVal", {})
            },
            // 放大地图
            enlarge() {
                this.map.setZoom(this.map.getZoom() + 1);
            },
            // 缩小地图
            lessen() {
                this.map.setZoom(this.map.getZoom() - 1);
            },
        }
    }
</script>

<style scope>
    .bigmap {
        position: absolute;
        top: 70px;
        bottom: 0;
        left: 100px;
        right: 0;
        z-index: 10;
        overflow: hidden;
    }
    #allmap {
        width: 100%;
        height: 100%;
        -webkit-transition: all 0.5s ease-in-out;
        transition: all 0.5s ease-in-out;
    }
 
    .btn {
        position: absolute;
        right: 0;
        top: 50px;
    }
    .BMapLib_circle,
    .BMapLib_polyline,
    .BMapLib_polygon,
    .BMapLib_rectangle {
        display: none
    }
    .BMapLib_Drawing_panel {
        margin-left: 30px
    }
</style>
