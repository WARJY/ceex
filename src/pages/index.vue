<template>
    <NavMain :iconfont="iconfont" :setting="navSetting" :styleDefault="styleDefault" :styleActive="styleActive">
        <div slot="index">
            <Page>
                <div style="height: 2000px;">
                    <Banner :setting="bannerSetting" />
                </div>
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
        props:{},
        data:function(){
            return {
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
            
        }
    }
</script>

<style scoped>


</style>