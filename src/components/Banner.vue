<template>
    <div>
        <scroller class="banner" :style="{width:deviceWidth + 'px'}" show-scrollbar="false" scroll-direction="horizontal" @scroll="handleScroll">
            <image class="image" v-for="(item,index) in setting" :src="item.src" :style="{width:deviceWidth + 'px'}" :ref="'image' + index"></image>
        </scroller>
        <div class="options" :style="{width:deviceWidth + 'px'}">
            <div v-for="(item,index) in setting" :class="['orb',index===currentIndex?'active':' ']" @click="handleOrb(index)"></div>
        </div>
    </div>

</template>

<script>
    const dom = weex.requireModule('dom')
    const modal = weex.requireModule('modal')
    export default {
        name: "banner",
        props: {
            setting: {
                type: Array,
                required: true,
                default: () => {
                    return []
                }
            }
        },
        data() {
            return {
                deviceWidth: 750,
                deviceHeight: 1334,
                currentIndex: 0
            }
        },
        methods: {
            handleScroll(e) {
            },
            handleOrb(index) {
                if(index) {
                    const el = this.$refs['image' + index][0]
                    dom.scrollToElement(el, {})
                    this.$data.currentIndex = index
                }
            }
        }
    }
</script>

<style scoped>
    .banner {
        height: 270px;
        flex-direction: row;
    }
    
    .image {
        height: 270px;
    }
    
    .options {
        flex-direction: row;
        justify-content: center;
        align-items: center;
        position: relative;
        bottom: 30px;
    }
    
    .orb {
        width: 14px;
        height: 14px;
        background-color: #999;
        opacity: 0.5;
        margin: 0 10px;
        border-radius: 50%;
    }
    
    .active {
        opacity: 1;
    }
</style>