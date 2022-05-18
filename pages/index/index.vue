<template>
	<view class="menu-content">
		<scroll-view
			scroll-y="true"
			class="menus"
			:scroll-into-view="menuScrollIntoView"
			scroll-with-animation
		>
			<view class="wrpper">
				<unicloud-db v-slot:default="{data, loading, error, options}" collection="uni-menu">
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
							<view class="dot" v-show="menuCartNum(item._id)">
								{{ menuCartNum(item._id) }}
							</view>
						</view>
					</view>
				</unicloud-db>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				menuScrollIntoView: '', 
				currentCateId: '' // 被选中的菜单id 
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	@import '@/pages/index/menu.scss';
</style>
