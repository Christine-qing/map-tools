# map-tools

> map-tools 地图工具

## vue cli搭建 ，UI框架iview，百度地图，包含vue的bus通信

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run e2e tests
npm run e2e

# run all tests
npm test
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
# map-tools


## 在 vue 中避免不了组件中的通信，无论是父子组件还是非父子组件，那我们今天就讲一个简单的组件通信，通过vue 中的bus来实现。

第一步我们需要在src的文件夹新建一个  ` bus.js `的文件,然后将vue引入并导出：
```
import Vue from 'vue'
export default new Vue()
```
接下来我们就要开始进行两组件的通信：

以下示例是从我写的小demo中抽出来的，我的目录中A组件和B组件同属于C组件下的子组件，A和B是同级关系。

我们的需求是，通过点击A组件中的三个不同的按钮，并依次传出三个不同的值，然后B组件接收3个值，根据这三个值来判断接下来的操作。

## A组件：
```
<template>
<div class="left">
<ul>
<li @click="download”>a</li>
<li @click="squaredUp">b</li>
<li @click="upload”>c</li>
</ul>
</div>
</template>

<script>
import bus from '../bus’ //引入我们上面创建的bus
export default {
    data() {
        return {
        }
    },
    mounted() {},
    methods: {
    // 第一个事件
        download() {
            bus.$emit("tabDisplay", 0)
        },

    // 第二个事件
        squaredUp() {
            bus.$emit("tabDisplay", 1)
        },

    // 第三个事件
        upload() {
            alert("第三个事件")
            },
        }
    }
</script>

```
## 在A组件中，我们将上面创建的文件引入，然后添加了3个事件，在前两个事件中都添加了一句话:
  ``` 
  bus.$emit("tabDisplay", 0)
```
就是这句话中的:
 bus.$emit()
就是我们需要的值进行发射，后面括号中的第一个值是我们发射出去的事件的名字，第二个值是我们发射时携带的值。

接下来我们就需要在B组件中进行接收了：
## B组件
```
<template>
    <div v-if="index===0”>当index===0时显示</div>
</template>

<script>
    import bus from '../bus'
    export default {
        data() {
            return {
                index: 1,
            }
        },
        mounted() {
            bus.$on("tabDisplay", this.tableDisplay)
        },
        methods: {
            tableDisplay(value) {
                this.index = value;
            }    

        }
    }
</script>

```
我们在上面的B组件中也是同样先讲bus引入，接下来在mounted中接收我们需要的事件
 ```
 bus.$on("tabDisplay", this.tableDisplay)
```
接收事件时，第一个参数时我们在发射时定义的名称，第二个值是我们本组件的方法，在methods中我们将接收到的值转化为我们想要的值，并加以利用.本示例是根据检测index的值来控制部门页面的显示与隐藏。

以上就是简洁的bus通信，如果还不太清楚可以在github上下载原本的小demo。
