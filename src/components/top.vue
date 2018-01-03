
<template>
    <div class="header">
        logo </div>
</template>

<script>
    import bus from '../bus'
    export default {
        data() {
            return {}
        },
        mounted() {
            bus.$on("love", this.abc)
        },
        methods: {
            abc(a, b) {
                // console.log("abc==="+a, b);
            }
        }
    }
</script>

<style scope>
    .header {
        position: fixed;
        width: 100%;
        height: 70px;
        background: #0099FF;
        border-bottom: 1px solid white;
        color: white;
    }
</style>