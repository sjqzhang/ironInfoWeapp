<template>
	<view>
		<scroll-view class="scontent" scroll-x>
			<!-- #ifndef H5 -->
			<uParse :content="content" />
			<!-- #endif -->
			<!-- #ifdef H5 -->
			<view class="content" v-html="content">
			</view>
			<!-- #endif -->
		</scroll-view>
		<!--  #ifdef  MP-WEIXIN -->
		<ad unit-id="adunit-299bbc0df0e741aa"></ad>
		<!-- #endif -->
		<!-- #ifndef MP-WEIXIN -->
		<!-- 广告 -->
		<view class="iron-contact">
			<view class="info-text">感觉价格不合理？ 欢迎联系我们议价</view>
			<view class="info-text">联系电话(点击即可拨打)</view>
			<view class="phone"><view @click="call('17625456779')">17625456779</view><view @click="call('13856262575')">13856262575</view></view>
		</view>
		<view class="qrcodes">
			<image class="qrcode" src="https://s1.ax1x.com/2018/12/02/FuDQVP.jpg"></image>
			<image class="qrcode" src="https://s1.ax1x.com/2018/12/02/FuDKbt.md.jpg"></image>
		</view>
		<!-- #endif -->
	</view>
</template>

<script>
	import marked from '../../components/marked'
	// #ifndef H5
	import uParse from '../../components/uParse/src/wxParse.vue'
	// #endif
	export default {
		name: 'articlePage',
		components: {
			// #ifndef H5
			uParse
			// #endif
		},
		data() {
			return {
				title: '',
				content: ''
			};
		},
		onShareAppMessage() {
			return {
				title: '钢材信息文章',
				path: '/pages/article/article'
			}
		},
		onLoad(option) {
			try {
				const value = uni.getStorageSync('article');
				if(option.type === 'news') {
					this.content = value
					return
				}
				if (value) {
					const result = JSON.parse(value)
					this.title = result.title
					// #ifndef H5
					this.content = marked(result.content)
					// #endif
					
					// #ifdef H5
					this.content = marked(result.content)
					// #endif
				}
			} catch (e) {
				console.log(e)
				// error
			}
		},
		destroyed(){
			try {
				uni.removeStorageSync('article');
			} catch (e) {
				// error
			}
		}
	}
</script>

<style>
	@import url("../../components/uParse/src/wxParse.css");
	.header {
		height: 100px;
	}
	.iron-contact {
		text-align: center;
	}
	.qrcodes {
		text-align: center;
	}
	.qrcodes .qrcode {
		width: 200rpx;
		height: 200rpx;
	}
</style>
