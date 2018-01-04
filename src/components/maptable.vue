<template>
    <div id="table" v-if="index===0">
        <Table stripe :columns="columns" :data="setData" height="350" class="tab" ref=table no-data-text="抱歉，请去地图打点"></Table>
        <Button type="primary" size="large" @click="exportData(1)" class="latBtn">
              <Icon type="ios-download-outline"></Icon> 经纬度导出
             </Button>
        <!-- <Button type="primary" size="large" @click="exportData(2)" class="lngBtn">
               <Icon type="ios-download-outline"></Icon> 纬经度导出
               </Button> -->
    </div>
</template>

<script>
    import bus from '../bus'
    export default {
        data() {
            return {
                index: 1,
                tableValue: [],
                columns: [{
                        title: 'name',
                        key: "name"
                    },
                    {
                        title: 'lat',
                        key: 'lat'
                    },
                    {
                        title: 'lng',
                        key: 'lng'
                    }
                ],
                setData: []
            }
        },
        mounted() {
            // bus.$on("love", this.abc)
            bus.$on("tabDisplay", this.tableDisplay)
            bus.$on("tabVal", this.tableVal)
        },
        methods: {
            tableDisplay(value) {
                this.index = value;
            },
            // 将数据添加到表格
            tableVal(point) {
                if (point.lat) {
                    this.setData.push({
                        name: this.setData.length + 1,
                        lat: point.lat,
                        lng: point.lng
                    });
                } else {
                    this.setData = [];
                }
            },
            //导出数据 
            exportData(type) {
                if (type === 1) {
                    this.$refs.table.exportCsv({
                        filename: '经纬度导出',
                    });
                }
                else if (type === 2) {
                    this.$refs.table.exportCsv({
                        filename: '纬经度导出',
                        columns: 111,
                    });
                }
            }
        }
    }
</script>

<style scope>
    #table {
        width: 351px;
        height: 400px;
        background: white;
        border: 1px solid #ccc;
        position: absolute;
        top: 70px;
        right: 60px;
        z-index: 20;
    }
    .tab {
        z-index: 20;
    }
    .latBtn {
        position: absolute;
        bottom: 6px;
        right: 50px;
        z-index: 21;
    }
    .lngBtn {
        position: absolute;
        bottom: 0;
        right: 20px;
        z-index: 21;
    }
</style>