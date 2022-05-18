<template>
	<view class="container">
		<view class="main">
			<view class="content">
				<scroll-view
					scroll-y="true"
					class="menus"
					:scroll-into-view="menuScrollIntoView"
					scroll-with-animation
				>
					<view class="wrpper">
						<unicloud-db
							v-slot:default="{data, loading, error, options}"
							collection="milk-menu"
							class="milk-menu"
						>
							<view v-if="error">{{error.message}}</view>
							<view v-else>
								<view
									class="menu"
									v-for="item in data"
									:id="`menu-${item._id}`"
									:class="{ 'current': item.id === currentCateId }"
									:key="index"
									@tap="handleMenuTap(item._id)"
								>
									<text>{{ item.name }}</text>
									<!-- <view class="dot" v-show="menuCartNum(item._id)">
										{{ menuCartNum(item._id) }}
									</view> -->
								</view>
							</view>
						</unicloud-db>
					</view>
				</scroll-view>
				<!-- goods list begin -->
				<scroll-view
					class="goods"
					scroll-y
					scroll-with-animation
					:scroll-top="cateScrollTop"
					@scroll="handleGoodsScroll"
				>
					<view class="wrpper">
						<swiper
							class="ads"
							id="ads"
							autoplay
							:indicator-dots="true"
							:interval="3000"
							:duration="1000"
						>
							<swiper-item v-for="(item, index) in ads" :key="adsIndex">
								<image :src="item.image"></image>	
							</swiper-item>
						</swiper>
						<view class="list">
							<view class="category"
								v-for="(item, index) in goods"
								:key="index"
								:id="`cate-${item._id}`"
							>
								<view class="title">
									<text>{{ item.name }}</text>
									<image :src="item.icon" class="icon"></image>
								</view>
								<view class="items">
									<!-- 商品begin -->
									<view class="good" v-for="(good, key) in item.goods_list" :key="key">
										<image :src="good.images" class="image" mode=""></image>
										<view class="right">
											<text class="name">{{ good.name }}</text>
											<text class="tips">{{ good.content }}</text>
											<view class="price_and_action">
												<text class="price">￥{{ good.price }}</text>
												<view class="btn-group" v-if="good.use_property">
													<button type="primary" class="btn property_btn"
														hover-class="none" size="mini"
													 >选规格</button>
													 <!-- <view class="dot" v-show="goodCartNum(good._id)">
													 	{{ goodCartNum(good._id) }}
													 </view> -->
												</view>
												<view class="btn-group" v-else>
													<button type="primary" class="btn add_btn" size="mini"
														hover-class="none"
													>
														<view class="iconfont iconadd-select"></view>
													</button>
												</view>
											</view>
										</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ads: [
					{image: 'https://img-shop.qmimg.cn/s23107/2020/04/27/4ebdb582a5185358c4.jpg?imageView2/2/w/600/h/600'},
					{image: 'https://images.qmai.cn/s23107/2020/05/08/c25de6ef72d2890630.png?imageView2/2/w/600/h/600'},
					{image: 'https://img-shop.qmimg.cn/s23107/2020/04/10/add546c1b1561f880d.jpg?imageView2/2/w/600/h/600'},
					{image: 'https://images.qmai.cn/s23107/2020/04/30/b3af19e0de8ed42f61.jpg?imageView2/2/w/600/h/600'},
					{image: 'https://img-shop.qmimg.cn/s23107/2020/04/17/8aeb78516d63864420.jpg?imageView2/2/w/600/h/600'},
				],
				menuScrollIntoView: '', 
				currentCateId: '' ,// 被选中的菜单id,
				goods: [] // 用函数去请求，不用直接展示的标签商品
			}
		},
		computed: {
		},
		onShow() {
			this.get_goods();
		},
		methods: {
			async get_goods() {
				let good_list = []
				const db =  uniCloud.database(); //代码块为cdb
				const goods = db.collection('milk-goods').getTemp()
				const property_child = db.collection("milk-property-child").getTemp()
				const property = db.collection('milk-property').getTemp()
				db.collection('milk-goods-and-property', goods, property)
				.get()
				.then(res=> {
					console.log(res);
					if(res.success) {
						const { data } = res.result
						// 拿到多个数据，以商品为条件分类出可以有多个东西
						data.forEach(item => {
							
						})
					}
				})
			},
			handleMenuTap(_id) {
				console.log(_id)
			},
			handleGoodsScroll({detail}) {
				// 商品列表滚动事假
				const { scrollTop } = detail;
				let tabs = this.goods.filter(item => item.top <= scrollTop).reverse();
				if(tabs.length) {
					this.currentCateId = tab[0]._id
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import 'menu.scss';
</style>
