
<template>
    <div id="table" v-if="index===0">
        <Table stripe :columns="columns1" :data="data1" height="350" class="tab" ref=table no-data-text="抱歉，请去地图打点"></Table>
        <Button type="primary" size="large" @click="exportData(1)" class="latBtn">
                                   <Icon type="ios-download-outline"></Icon> 
                                  经纬度导出
                            </Button>
        <!-- <Button type="primary" size="large" @click="exportData(2)" class="lngBtn">
                                  <Icon type="ios-download-outline"></Icon>
                                   纬经度导出
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
                columns1: [{
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
                data1: [
                    // {
                    //     name:'',
                    //     lat :'',
                    //     lng: ''
                    // },
                ]
            }
        },
        mounted() {
            // bus.$on("love", this.abc)
            bus.$on("tabDisplay", this.tableDisplay)
            bus.$on("tabVal", this.tableVal)
        },
        methods: {
            tableDisplay(d) {
                this.index = d;
            },
            // 将数据添加到表格
            tableVal(point) {
                debugger
                // this.tableValue = point;
                // point.forEach((item, index, arr) => {
                //     this.data1.push({
                //         name: index,
                //         lat: item.point.lat,
                //         lng: item.point.lng
                //     })
                // })
                if (point.lat) {
                    this.data1.push({
                        name: this.data1.length + 1,
                        lat: point.lat,
                        lng: point.lng
                    });
                } else {
                    this.data1 = [];
                }
            },
            //导出数据 
            exportData(type) {
                // debugger
                if (type === 1) {
                    this.$refs.table.exportCsv({
                        filename: '经纬度导出',
                    });
                }
                // console.log(this.$refs.table.exportCsv)
                else if (type === 2) {
                    this.$refs.table.exportCsv({
                        filename: '纬经度导出',
                        columns1: 111,
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