<template>
    <scroller class="page" :style="{width:deviceWidth + 'px',minHeight:deviceHeight + 'px'}" @loadmore="handleLoading">
        <refresh @refresh="handleRefresh" :display="refreshing ? 'show' : 'hide'">
            <text class="refresh">正在刷新</text>
        </refresh>
        <slot></slot>
        <loading @loading="handleLoading" :display="loading ? 'show' : 'hide'">
            <text class="loading">正在加载</text>
        </loading>
    </scroller >
</template>

<script>
    export default{
        name:"page",
        props:{},
        data(){
            return {
                deviceWidth:750,
                deviceHeight:1334,
                refreshing:false,
                loading:false
            }
        },
        created(){
            this.$data.deviceWidth = WXEnvironment.deviceWidth
            this.$data.deviceHeight = WXEnvironment.deviceHeight
        },
        methods:{
            handleRefresh(e){
                this.$data.refreshing = true
                setTimeout(()=>{
                    this.$data.refreshing = false
                },2000)
            },
            handleLoading(e){
                this.$data.loading = true
                setTimeout(()=>{
                    this.$data.loading = false
                },2000)
            }
        }
    }
</script>

<style scoped>
    .page{
        background-color: #f5f5f5;
    }
    .refresh{
        text-align: center;
        width: 750px;
    }
    .loading{
        text-align: center;
        width: 750px;
    }
</style>