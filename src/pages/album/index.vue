<template>
<view>专辑详情</view>
</template>
<script >
export default {
    data(){
        return{
            params:{
                limit:30,
                order:"new",
                skip:0,
                first:0
            },
            id:-1,
            album:{},
            wallpaper:[]
        }
    },
    //
    onLoad(options){
        //console.log(options);
        this.id=options.id;
        this.getList();
    },
    methods:{
        getList(){
            this.request({
                url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`, //仅为示例，并非真实接口地址。
                data: this.params
            })
            .then(result=>{
                console.log(result);
                this.album=result.res.album;
                this.wallpaper=result.res.wallpaper;
            })
        }
    }
}
</script>
<style lang='scss'>
</style>