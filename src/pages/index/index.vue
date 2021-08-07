<template>
  <view class="index">
    <view class="tabs">
      <view class="tab" v-for="(item,index) in tabs" :key="index"   :class="now_tab == index ? 'on':''" @tap="changeTab(index)">{{item}}</view>
    </view>
      <view class="list" v-if="now_tab == 0">
        <view class="item_box" v-for="(item,index) in data_a" :key="item.id">
          
        <view class="item">
          <view class="txt">{{item.content}}</view>
          <view class="meta">
            <view class="copy" @tap="copy(item.content)">一键复制</view>
          </view>
        </view>
        <view class="ad_item" v-if="index == 0 || index == 6 || index == 12 || index == 18"> 
            <ad unit-id="adunit-13067db7c8791342"></ad>
          </view>
          <view class="ad_item" v-if="index == 3 || index == 9 || index == 15 || index == 20">
            <ad unit-id="adunit-45ad040b4d9fcbfa" ad-type="video" ad-theme="white"></ad>
          </view>
        </view>
        <view class="load_all" v-if="loadAll_a">没有更多了</view>
      </view>
      <view class="list" v-if="now_tab == 1">
        
        <view class="item_box" v-for="(item,index) in data_b" :key="item.id">
          
        <view class="item">
          <view class="txt">{{item.content}}</view>
          <view class="meta">
            <view class="copy" @tap="copy(item.content)">一键复制</view>
          </view>
        </view>
        <view class="ad_item" v-if="index == 0 || index == 6 || index == 12 || index == 18"> 
            <ad unit-id="adunit-13067db7c8791342"></ad>
          </view>
          <view class="ad_item" v-if="index == 3 || index == 9 || index == 15 || index == 20">
            <ad unit-id="adunit-45ad040b4d9fcbfa" ad-type="video" ad-theme="white"></ad>
          </view>
        </view>
        <view class="load_all" v-if="loadAll_b">没有更多了</view>
      </view>
      <view class="list" v-if="now_tab == 2">
        
        <view class="item_box" v-for="(item,index) in data_c" :key="item.id">
          
        <view class="item">
          <view class="txt">{{item.content}}</view>
          <view class="meta">
            <view class="copy" @tap="copy(item.content)">一键复制</view>
          </view>
        </view>
        <view class="ad_item" v-if="index == 0 || index == 6 || index == 12 || index == 18"> 
            <ad unit-id="adunit-13067db7c8791342"></ad>
          </view>
          <view class="ad_item" v-if="index == 3 || index == 9 || index == 15 || index == 20">
            <ad unit-id="adunit-45ad040b4d9fcbfa" ad-type="video" ad-theme="white"></ad>
          </view>
        </view>
        <view class="load_all" v-if="loadAll_c">没有更多了</view>
      </view>
  </view>
</template>

<script>
import Taro from "@tarojs/taro";
import { serverUrl } from "../../utils/config";
import './index.less'

export default {
  data () {
    return {
      tabs: ['朋友圈文案','邀请函文案','婚礼致辞'],
      now_tab: 0,
      page_a: 1,
      page_b: 1,
      page_c: 1,
      data_a: [],
      data_b: [],
      data_c: [],
      loading: false,
      loadAll_a: false,
      loadAll_b: false,
      loadAll_c: false
    }
  },
  onShareAppMessage() {},
  onShareTimeline() {
    return {
      title: "最新最全的婚礼文案",
      imageUrl: "",
      query: "",
    };
  },
  mounted(){
    this.getDataA()
  },
  onPullDownRefresh() {
    if(this.now_tab == 0){
      this.page_a = 1
      this.getDataA();
    }else if(this.now_tab == 1){
      this.page_b = 1
      this.getDataB();
    }else{
      this.page_c = 1
      this.getDataC();
    }
  },
  onReachBottom() {
    if(this.now_tab == 0){
      this.getDataA();
    }else if(this.now_tab == 1){
      this.getDataB();
    }else{
      this.getDataC();
    }
  },
  methods: {
    copy(data){
      Taro.setClipboardData({
        data: data,
        success: function (res) {
          Taro.getClipboardData({
            success: function (res) {
              wx.showModal({
                title: "提示",
                content: "内容复制成功",
                showCancel: false,
              });
            }
          })
        }
      })
    },
    changeTab(tab){
      this.now_tab = tab
      if(this.now_tab == 0){
        this.page_a = 1
        this.getDataA();
      }else if(this.now_tab == 1){
        this.page_b = 1
        this.getDataB();
      }else{
        this.page_c = 1
        this.getDataC();
      }
    },
    getDataA(){
      if (!this.loading) {
        this.loading = true;
        Taro.showLoading({
          title: '加载中',
        })
        Taro.request({
          url: serverUrl + '?c=books&type='+this.now_tab+'&page=' + this.page_a,
        }).then((res) => {
          Taro.hideLoading()
          Taro.stopPullDownRefresh()
          if (res.data.errcode == 0) {
            if (this.page_a == 1) {
              this.data_a = res.data.data;
              this.page_a++;
            } else {
              if(res.data.data.length > 0){
                this.data_a = [...this.data_a, ...res.data.data];
                this.page_a++;
              }else{
                this.loadAll_a = true
              }
            }
          }
          this.loading = false;
        });
      }
    },
    getDataB(){
      if (!this.loading) {
        this.loading = true;
        Taro.showLoading({
          title: '加载中',
        })
        Taro.request({
          url: serverUrl + '?c=books&type='+this.now_tab+'&page=' + this.page_b,
        }).then((res) => {
          Taro.hideLoading()
          Taro.stopPullDownRefresh()
          if (res.data.errcode == 0) {
            if (this.page_b == 1) {
              this.data_b = res.data.data;
              this.page_b++;
            } else {
              if(res.data.data.length > 0){
                this.data_b = [...this.data_b, ...res.data.data];
                this.page_b++;
              }else{
                this.loadAll_b = true
              }
            }
          }
          this.loading = false;
        });
      }
    },
    getDataC(){
      if (!this.loading) {
        this.loading = true;
        Taro.showLoading({
          title: '加载中',
        })
        Taro.request({
          url: serverUrl + '?c=books&type='+this.now_tab+'&page=' + this.page_c,
        }).then((res) => {
          Taro.hideLoading()
          Taro.stopPullDownRefresh()
          if (res.data.errcode == 0) {
            if (this.page_c == 1) {
              this.data_c = res.data.data;
              this.page_c++;
            } else {
              if(res.data.data.length > 0){
                this.data_c = [...this.data_c, ...res.data.data];
                this.page_c++;
              }else{
                this.loadAll_a = true
              }
            }
          }
          this.loading = false;
        });
      }
    },
  }
}
</script>
