<template>
    <NavMain :iconfont="iconfont" :setting="navSetting" :styleDefault="styleDefault" :styleActive="styleActive">
        <div slot="index">
            <Page :refreshing="refreshing" @refreshing="handleRefresh" :loading="loading" @loadining="handleLoading">
                <Banner :setting="bannerSetting" />
                <div style="height: 1500px;background-color: #333;"></div>
            </Page>
        </div>
        <div slot="my">
            <Page>
                <div>222</div>
            </Page>
        </div>
    </NavMain>
</template>

<script>
    import Page from "@/components/Page"
    import NavMain from "@/components/NavMain"
    import Banner from "@/components/Banner"
    export default{
        name:"index",
        components:{
            Page:Page,
            NavMain:NavMain,
            Banner:Banner
        },
        data:function(){
            return {
                refreshing:false,
                loading:false,
                iconfont:"//at.alicdn.com/t/font_811848_qrm8hrhlfkg.ttf",
                navSetting:[
                    {name:"index",icon:"\ue751;",title:"首页"},
                    {name:"my",icon:"\ue75e;",title:"我的"}
                ],
//              navSetting:[
//                  {icon:"https://vuejs.org/images/logo.png",activeIcon:" ",title:"首页"},
//                  {icon:"https://vuejs.org/images/logo.png",activeIcon:" ",title:"首页"}
//              ]
                styleDefault:{
                    icon:{
                        color:'#333'
                    },
                    title:{
                        color:'#333'
                    }
                },
                styleActive:{
                    icon:{
                        color:'#00d2ff'
                    },
                    title:{
                        color:'#00d2ff'
                    }
                },
                bannerSetting:[
                    {src:"https://vuejs.org/images/logo.png"},
                    {src:"https://vuejs.org/images/logo.png"},
                    {src:"https://vuejs.org/images/logo.png"}
                ]
            }
        },
        methods:{
            handleRefresh:function(){
                this.$data.refreshing = true
                setTimeout(()=>{
                    this.$data.refreshing = false
                },1000)
            },
            handleLoading:function(){
                this.$data.loading = true
                setTimeout(()=>{
                    this.$data.loading = false
                },1000)
            }
        }
    }
</script>

<style scoped>


</style>