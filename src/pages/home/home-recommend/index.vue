<template>
  <scroll-view @scrolltolower="handleTolower" class="recommend_view" scroll-y v-if="recommend.length>0">

     <view class="recommend_wrap">
       <view class="recommend_item" v-for="item in recommend" :key="item.id">
         <image :src="item.thumb" mode="widthFix" />
       </view>
       </view>
         <view class="monthes_wrap">
      <view class="monthes_title">
        <view class="monthes_title_info">
          <view class="monthes_info">
            <text> {{monthes.DD}} / </text>
            {{monthes.MM}} 月
          </view>
          <view class="monthes_text"> {{monthes.title}} </view>
        </view>
        <view class="monthes_title_more">更多 > </view>
      </view>
      <view class="monthes_content">
        <view
          class="monthes_item"
          v-for="(item,index) in monthes.items"
          :key="item.id"  
        >
        <go-detail :list="monthes.items" :index="index" >
          <image
            mode="aspectFill"
            :src="item.thumb+item.rule.replace('$<Height>',360)"
          ></image>
          </go-detail>
        </view>
      </view>
    </view>

       <view class="hots_wrap">
         <view class="hots_title">
           <text>热门</text>
         </view>
         <view class="hots_content">
           <view class="hots_item" v-for="item in hots" :key="item.id">
             <image :src="item.thumb" mode="widthFix" />
           </view>
         </view>
       </view>
     
  </scroll-view>
 
</template>

<script>
import  moment from "moment";
export default {
  data(){
    return{
      recommend:[],
      monthes:{},
      hots:[],
      params:{
        limit: 30,
        order:"hot",
        skip:0
      },
      hasMore:true
    }

  },
  mounted(){
  
  
  
    this.getList();
  },
  methods:{
    handleTolower(){
       if(this.hasMore){
      this.params.skip+=this.params.limit;
      this.getList();
       }else{
         uni.showToast({
           title: '没有数据了',
           duration: 2000,
           icon:"none"
         });
       }
    
    },
    getList(){
      this.request({
      url: 'http://157.122.54.189:9088/image/v3/homepage/vertical', //仅为示例，并非真实接口地址。
      data: this.params
    })
    .then(result=>{
      if(result.res.vertical===0){
        this.hasMore=false;
        return;
      }
      if(this.recommend.length===0){
      this.recommend=result.res.homepage[1].items;
      this.monthes=result.res.homepage[2];
      this.monthes.MM=moment(this.monthes.stime).format("MM")
      this.monthes.DD=moment(this.monthes.stime).format("DD")
     
      }
      this.hots=[...this.hots,...result.res.vertical]
    
      
    })
    }

  }
}
</script>

<style lang="scss" scope>
.recommend_view{
  height:  calc(100vh - 36px);
}
.recommend_wrap{
  display: flex;
  flex-wrap: wrap;
 
  .recommend_item{
    width:50%;
    border: 3rpx solid #fff;
  }
}
.monthes_wrap {
  .monthes_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .monthes_title_info {
      color: $color;
      font-size: 30rpx;
      font-weight: 600;
      display: flex;
      .monthes_info {
        text {
          font-size: 36rpx;
        }
      }

      .monthes_text {
        font-size: 34rpx;
        color: #666;
        margin-left: 30rpx;
      }
    }

    .monthes_title_more {
      font-size: 24rpx;
      color: $color;
    }
  }

  .monthes_content {
    display: flex;
    flex-wrap: wrap;
    .monthes_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}
.hots_wrap {
  
  .hots_title {
    padding: 20rpx;
    text {
      border-left: 20rpx solid $color;
      padding-left: 20rpx;
      font-size: 34rpx;
      font-weight: 600;
    }
  }

  .hots_content {
    display:flex;
    flex-wrap: wrap;
    .hots_item {
      width: 33.33%;
      border: 1rpx solid #fff;
      image {
      }
    }
  }
}
</style>