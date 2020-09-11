<template>
  <view>
    <!-- 专辑背景开始 -->
    <view class="album_bg">
      <image mode="widthFix" :src="album.cover"></image>
      <view class="album_info">
        <view class="album_name">{{album.name}}</view>
        <view class="album_btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑背景结束 -->
    <!-- 传记作者开始 -->
    <view class="album_author">
      <view class="album_author_info">
        <image mode="widthFix" :src="avatar"></image>
        <view class="author_name">{{album.user.name}}</view>
      </view>
      <view class="album_author_desc">
        <!-- text 能识别 换行符 -->
        <text>{{album.desc}}</text>
        </view>
    </view> 
    <!-- 传记作者结束 -->
    <!-- 列表开始 -->
    <view class="album_list">
      <view class="album_item" v-for="(item, index) in wallpaper" :key="item.id">
        <goods-detail :list="wallpaper" :index="index">
          <image mode="widthFix" :src="item.thumb + item.rule.replace('$<Height>, 360')"></image>
        </goods-detail>
      </view>
    </view>
    <!-- 列表结束 -->
  </view>
  
</template>

<script>
import { goodsDetail } from '@/components/goodsDetail'
export default {
  data() {
    return {
      params: {
        limit: 30,
        order: "new",
        skip: 0,
        first: 1
      },
      id: '',
      album: {},
      wallpaper: [],
      avatar: "data:image/jpeg;base64,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",
      hasMore: true
    }
  },
  components: {
    goodsDetail
  },
  onLoad(options) {
    this.id = options.id
    this.getList()
  },
  // 页面触底事件
  onReachBottom() {
    if (this.hasMore) {
      this.params.first = 0
      this.params.skip += this.params.limit
      this.getList()
    } else {
      uni.showToast({
        title: "没有下一页了",
        icon: "none"
      })
    }
  },
  methods: {
    getList() {
      this.request({
        url: `https://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(res => {
        if (Object.keys(this.album).length === 0) {
          this.album = res.res.album
        }
        if (res.res.wallpaper.length === 0) {
          this.hasMore = false
          return
        }
        this.wallpaper = [...this.wallpaper, ...res.res.wallpaper]
      })
    }
  },
}
</script>

<style lang="scss" scoped>
  .album_bg {
    position: relative;
    image {

    }
    .album_info {
      position: absolute;
      width: 100%;
      left: 0;
      bottom: 0;
      display: flex;
      justify-content: space-between;
      height: 80rpx;
      color: #fff;
      align-items: center;
      padding: 0 15rpx;
      .album_name {
        font-size: 40rpx;
      }
      .album_btn{
        background-color: $color;
        width: 152rpx;
        height: 60rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 10rpx;
      }
    }
  }

  .album_author {
    padding: 10rpx;
    .album_author_info {
      display: flex;
      padding: 10rpx 0;
      image {
        width: 50rpx;
      }
      .author_name {
        margin-left: 15rpx;
        color: #000;
      }
    }
    .album_author_desc {}
  }

  .album_list {
    display: flex;
    .album_item {
      width: 33.33%;
      border: 3rpx solid #fff;
      image {

      }
    }
  }
</style>
