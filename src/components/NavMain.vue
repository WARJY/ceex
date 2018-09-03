<template>
    <div>
        <div class="nav-content" :style="{width:deviceWidth + 'px',minHeight:deviceHeight + 'px'}">
            <div v-for="(item,index) in setting">
                <slot v-if="index===currentIndex" :name="item.name"></slot>
            </div>
        </div>
        <div class="nav-main" :style="{width:deviceWidth + 'px'}">
            <!--字体图标-->
            <div v-if="iconfont"  v-for="(item,index) in setting" class="nav-item" v-on:click="handleSwitch(index)">
                <text :class="['icon',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.icon:styleDefault.icon]">{{item.icon}}</text>
                <text :class="['title',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.title:styleDefault.title]">{{item.title}}</text>
            </div>
            <!--图片图标-->
            <div v-if="!iconfont" v-for="(item,index) in setting" class="nav-item" v-on:click="handleSwitch(index)">
                <image class="icon" :src="index===currentIndex?item.activeIcon:item.icon" :style="[index===currentIndex?styleActive.icon:styleDefault.icon]"></image>
                <text :class="['title',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.title:styleDefault.title]">{{item.title}}</text>
            </div>
        </div>
    </div>
</template>

<script>
    const domModule = weex.requireModule('dom')
    export default {
        name:'navMain',
        props:{
            setting:{
                type:Array,
                required:true
            },
            defaultIndex:Number,
            iconfont:String,
            styleDefault:{
                type:Object,
                default:function(){
                    return {
                        icon:{},
                        title:{}
                    }
                }
            },
            styleActive:{
                type:Object,
                default:function(){
                    return {
                        icon:{},
                        title:{}
                    }
                }
            }
        },
        data(){
            return {
                currentIndex:0,
                deviceWidth:750,
                deviceHeight:1334
            }
        },
        created(){
            if(this.iconfont){
                domModule.addRule('fontFace', {
                    'fontFamily': "iconfont",
                    'src': "url('" + this.iconfont + "')"
                });
            }
            if(this.defaultIndex) this.$data.currentIndex = this.defaultInde
            this.$data.deviceHeight = WXEnvironment.deviceHeight
        },
        methods:{
            handleSwitch(index){
                if(index>=0 && this.$data.currentIndex>=0){
                    if(index !== this.$data.currentIndex){
                        this.$data.currentIndex = index
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .nav-content{
        background-color: #F5F5F5;
    }
    .nav-main{
        height: 98px;
        background-color: #fff;
        border-top-width: 1px;
        border-top-color: #999;
        position: fixed;
        bottom: 0;
        z-index: 1000;
        align-items: center;
        justify-content: center;
        flex-direction: row;
    }
    .nav-placeholer{
        height: 98px;
    }
    .nav-item{
        align-items: center;
        justify-content: center;
        flex: 1;
    }
    .icon{
        font-family: iconfont;
        font-size: 44px;
        width: 44px;
        height: 44px;
    }
    .title{
        font-size: 20px;
    }
</style>