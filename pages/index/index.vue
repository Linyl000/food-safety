<template>
	<view class="pages">
		<u-swiper :list="list3" indicator indicatorMode="dot" circular height="160"></u-swiper>
		<div class="announcementt">
			<div class="top">
				<u-tag text="公示信息" type="error"></u-tag>
				<div>更多></div>
			</div>
			<u--text
				lineHeight="36"
				:lines="1"
				text="最础最重要的组件，故取名uView，表达源于uni-app和Vue之意，同时在此也对它示感谢。"
				@click="goArticle()"
			></u--text>
			<u--text
				:lines="1"
				text="最础最重要的组件，故取名uView，表达源于uni-app和Vue之意，同时在此也对它示感谢。"
				@click="goArticle()"
			></u--text>
		</div>
		<div class="has-token">
			<u-line></u-line>
			<div class="owner">
				<img :src="userInfo.logo" alt="" />
				<div class="owner-text">
					<div>
						{{ userInfo.nickName }}
						<span style="font-size: 26rpx;font-weight: 400;color: #F46F6F;margin:0 18rpx;">已认证</span>
						<span style="font-size: 26rpx;font-weight: 400;color: #01B578;margin:0 18rpx;">未认证</span>
					</div>
					<div style="font-size: 30rpx;font-weight: 400;">{{ linshiJylx }}</div>
				</div>
			</div>
			<div style="font-size: 36rpx;line-height: 1.5;margin-left: 24rpx;font-weight: 600;">商店管理</div>
			<div class="can-do">
				<div
					class="do-des"
					v-for="i in 4"
					:key="i"
					@click="goOtherPage(i)"
					:style="'background: ' + (i === 0 ? '#e8f3fe' : i === 1 ? '#FEF4F2' : i === 2 ? '#E4E9FD' : '#EBFEFF')"
				>
					<div class="des-left">
						<div class="title">{{ i === 0 ? '人员管理' : i === 1 ? '设备管理' : i === 2 ? '企业评价' : '行政监督' }}</div>
						<div v-if="i === 0 || i === 1" class="instructions">{{ i === 1 ? '设备数量：' + i : '人员数量：' + i }}</div>
					</div>
					<img
						:src="
							i === 0 ? '../../static/P1.png' : i === 1 ? '../../static/P2.png' : i === 2 ? '../../static/P3.png' : '../../static/P4.png'
						"
						alt=""
					/>
				</div>
			</div>
		</div>
	</view>
</template>

<script>
import { mapGetters, mapMutations, mapState } from 'vuex';
import { userGet } from '@/api/user.js';
export default {
	computed: {
		...mapState(['token'])
	},
	data() {
		return {
			userInfo: {},
			list3: [
				'https://cdn.uviewui.com/uview/swiper/swiper3.png',
				'https://cdn.uviewui.com/uview/swiper/swiper2.png',
				'https://cdn.uviewui.com/uview/swiper/swiper1.png'
			],
			jylxList: []
		};
	},
	onLoad() {
		this.getList();
		this.userGet();
	},
	computed: {
		linshiJylx() {
			const item = this.jylxList.find(item => item.id === this.userInfo.businessTypeId);
			return item ? item.key : '';
		}
	},
	methods: {
		getList() {
			this.jylxList = [{ key: 'a', id: '1' }, { key: 'b', id: '2' }, { key: 'c', id: '3' }, { key: 'd', id: '4' }];
		},
		userGet() {
			userGet().then(res => {
				if (!res.code === 0) {
					uni.$u.toast(res.msg);
					return;
				}
				uni.setStorageSync('userInfo', res.data);
				this.userInfo = res.data;
			});
		},

		goArticle() {},
		goOtherPage(i) {
			if (i === 3) {
				uni.navigateTo({
					url: '/pages_other1/supervise/supervise'
				});
			} else {
				uni.showToast({
					icon: 'none',
					title: '暂时未开放该功能~'
				});
			}
			if (i === 0) {
				uni.navigateTo({
					url: '/pages_other1/people/people'
				});
			} else if (i === 1) {
				uni.showToast({
					icon: 'none',
					title: '暂时未开放该功能~'
				});
			} else if (i === 2) {
				uni.navigateTo({
					url: '/pages_other1/evaluate/evaluate'
				});
			} else {
				uni.navigateTo({
					url: '/pages_other1/supervise/supervise'
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.pages {
	padding: 0 20rpx;
}
.announcementt {
	width: 718rpx;
	height: 232rpx;
	box-sizing: border-box;
	background: #f9f9f9;
	border-radius: 16rpx;
	margin: 24rpx 0;
	padding: 40rpx;
	.top {
		display: flex;
		justify-content: space-between;
	}
}
.owner {
	display: flex;
	align-items: center;
	padding: 40rpx 24rpx;
	img {
		width: 120rpx;
		height: 120rpx;
		margin-right: 24rpx;
	}
	.owner-text {
		font-size: 36rpx;
		font-weight: 600;
		line-height: 50rpx;
	}
}
.can-do {
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
	.do-des {
		display: flex;
		justify-content: space-between;
		margin-top: 20rpx;
		width: 348rpx;
		height: 172rpx;
		border-radius: 24rpx;
		box-sizing: border-box;
		padding: 30rpx;
		.title {
			font-size: 30rpx;
			font-weight: 600;
		}
		.instructions {
			font-size: 24rpx;
			font-weight: 400;
			line-height: 3;
		}
		img {
			width: 96rpx;
			height: 96rpx;
		}
	}
}
</style>
