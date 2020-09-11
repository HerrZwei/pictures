<template>
  <scroll-view @scrolltolower="handleToLower" :scroll-y="true" class="recommend_view" v-if="recommend.length > 0">
    <!-- 推荐开始 -->
    <view class="recommend_wrap">
      <view class="recommend_item" v-for="item in recommend" :key="item.id">
        <!-- <image :src="item.thumb" mode="widthFix"></image> -->
      </view>
    </view>
    <!-- 推荐结束 -->
    
    <!-- 月份开始 -->
    <view class="months_wrap">
      <view class="months_title">
        <view class="months_title_info">
          <view class="months_info">
            <text>{{months.DD}} / </text>
            {{months.MM}} 月
          </view>
          <view class="months_text">{{months.title}}</view>
        </view>
        <view class="months_title_more">更多 > </view>
      </view>
      <view class="months_content">
        <view class="months_item" v-for="(item, index) in months.items" :key="item.id">
          <goods-detail :list="months.items" :index="index">
            <image mode="aspectFill" :src="item.thumb + item.rule.replace('$<Height>', 360)"></image>
          </goods-detail>
        </view>
      </view>
    </view>
    <!-- 月份结束 -->
    <!-- 热门开始 -->
    <view class="hots_wrap">
      <view class="hots_title">
        <text>热门</text>
      </view>
      <view class="hots_content">
        <view class="hot_item"
          v-for="(item, index) in vertical" :key="item.id">
          <goods-detail :list="vertical" :index="index">
            <image mode="widthFix" :src="item.thumb"></image>
          </goods-detail>
        </view>
      </view>
    </view>
    <!-- 热门结束 -->
  </scroll-view>
</template>

<script>
import { moment } from 'moment'
import { goodsDetail } from '@/components/goodsDetail'
export default {
  data() {
    return {
      recommend: [],
      months: [],
      vertical: [],
      // 请求参数
      params: {
        limit: 30,
        order: "hot",
        skip: 0
      },
      hasMore: true
    }
  },
  components: {
    goodsDetail
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "推荐"
    })
    this.getList()
  },
  methods: {
    // 获取接口的数据
    getList() {
      this.request(
      {
        url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
        data: this.params
      }).then(res => {
        if (res.res.vertical.length === 0) {
          this.hasMore = false
          return
        }
        if (this.recommend.length === 0) {
          this.recommend = res.res.homepage[1].items
          this.months = res.res.homepage[2]
          this.vertical = res.res.vertical
          this.months.MM = moment(this.months.stime).format("MM")
          this.months.DD = moment(this.months.stime).format("DD")
        }
        this.vertical = [...this.vertical, ...res.res.vertical]
        
      })
    },
    handleToLower() {
      /*
        修改参数 skip += limit
      */
     if (this.hasMore) {
       this.params.skip += this.params.limit
       this.getList()
     } else {
       uni.showToast({
         title: "没有下一页了",
         icon: 'none'
       })
     }
    }
  },
}
</script>

<style lang="scss" scoped>
  .recommend_view {
    // 屏幕高度减去 头部标题的高度
    height: calc(100vh - 36px);

  }

  .recommend_wrap {
    display: flex;
    flex-wrap: wrap;
    .recommend_item {
      width: 50%;
      border: 5rpx solid white
    }
  }

  .months_wrap {
    .months_title {
      display: flex;
      justify-content: space-between;
      padding: 20rpx;
      .months_title_info {
        color: $color;
        font-size: 30rpx;
        font-weight: 600;
        display: flex;
        .months_info {
          text {
            font-size: 36rpx;
          }
        }
        .months_text {
          font-size: 34rpx;
          color: #666;
          margin-left: 30rpx;
        }
      }
      .months_title_more {
        font-size: 24rpx;
        color: $color;
      }
    }
    .months_content {
      display: flex;
      flex-wrap: wrap;
      .months_item {
        width: 33.33%;
        border: 5rpx solid white;
      }
    }
  }

  .hots_wrap {
    .hots_title {
      padding: 20rpx;
      text {
        border-left: 5rpx solid $color;
        font-size: 34rpx;
        font-weight: 600;
        padding-left: 20rpx;
      }
    }
    .hots_content {
      display: flex;
      flex-wrap: wrap;
      .hot_item {
        width: 33.33%;
        border: 5rpx solid white;
      }
    }
  }
</style>
