<template>
    <div>
        <div class="nav" ref="nav">
            <slot name="nav"></slot>
        </div>
        <div :style="{height:navHeight + 'px'}"></div>
        <scroller class="page" :style="{width:deviceWidth + 'px',minHeight:pageHeight + 'px'}" @loadmore="handleLoading">
            <refresh class="refresh" :style="{width:deviceWidth + 'px'}" @refresh="handleRefresh" :display="refreshing ? 'show' : 'hide'">
                <loading-indicator class="indicator"></loading-indicator>
            </refresh>
            <div :style="{minHeight:(pageHeight + 1) + 'px'}">
                <slot></slot>
                <div :style="{height:bottomHeight + 'px'}"></div>
            </div>
            <loading class="loading" :style="{width:deviceWidth + 'px',bottom:(bottomHeight + 1) + 'px'}" @loading="handleLoading" :display="loading ? 'show' : 'hide'">
                <loading-indicator class="indicator"></loading-indicator>
            </loading>
        </scroller >
    </div>
</template>

<script>
    import { Utils } from 'weex-ui';
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
            },
            navbarHeight:{
                type:Number,
                default:90
            }
        },
        data(){
            return {
                deviceWidth:750,
                deviceHeight:1334,
                pageHeight:1106,
                navHeight:90,
                bottomHeight:98
            }
        },
        created(){
            this.$data.deviceWidth = WXEnvironment.deviceWidth
            this.$data.deviceHeight = WXEnvironment.deviceHeight
            this.$data.bottomHeight = this.navbarHeight
        },
        ready(){
           let navHeight = this.$refs.nav.offsetHeight
           this.$data.navHeight = navHeight
           this.$data.pageHeight = this.$data.deviceHeight - navHeight - this.$data.bottomHeight - 40
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
    .nav{
        position: fixed;
        top: 0;
        z-index: 100;
    }
    .refresh{
        text-align: center;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        padding-top: 16px;
    }
    .loading{
        text-align: center;
        position: relative;
        flex-direction: row;
        align-items: center;
        justify-content: center;
    }
    .indicator {
        height: 40px;
        width: 40px;
        color: #00d2ff;
        margin-right: 10px;
    }
</style>