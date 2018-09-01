<template>
    <scroller class="page" :style="{width:deviceWidth + 'px',minHeight:deviceHeight + 'px'}" @loadmore="handleLoading">
        <refresh @refresh="handleRefresh" :display="refreshing ? 'show' : 'hide'">
            <text class="refresh">正在刷新</text>
        </refresh>
        <div :style="{minHeight:(deviceHeight + 1) + 'px'}">
            <slot></slot>
            <div style="height: 228px;"></div>
        </div>
        <loading @loading="handleLoading" :display="loading ? 'show' : 'hide'">
            <text class="loading">正在加载</text>
        </loading>
    </scroller >
</template>

<script>
    export default{
        name:"page",
        props:{
            refreshing:{
                type:Boolean,
                default:false
            },
            loading:{
                type:Boolean,
                default:false
            }
        },
        data(){
            return {
                deviceWidth:750,
                deviceHeight:1334
            }
        },
        created(){
            this.$data.deviceWidth = WXEnvironment.deviceWidth
        },
        methods:{
            handleRefresh(e){
                this.$emit("refreshing")
            },
            handleLoading(e){
                this.$emit("loadining")
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
        position: relative;
        bottom: 228px;
    }
</style>