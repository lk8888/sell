<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<div class="overview-title">
					<h1 class="name">{{seller.name}}</h1>
					<star :size="36" :score="seller.score"></star>
					<span class="rank-rate">({{seller.rankRate}}%)</span>
					<span class="sell-count">月售{{seller.sellCount}}单</span>
					<div class="collect">
						<i class="icon-favorite" :class="{active: isActive}" @click="collect"></i>
						<p class="text">{{activeText}}</p>
					</div>
				</div>
				<div class="overview-detail">
					<div class="minPrice item">
						<p class="text">起送价</p>
						<p class="price"><span class="num">{{seller.minPrice}}</span>元</p>
					</div>
					<div class="deliveryPrice item">
						<p class="text">商家配送</p>
						<p class="price"><span class="num">{{seller.deliveryPrice}}</span>元</p>
					</div>
					<div class="deliveryTime item">
						<p class="text">平均配送时间</p>
						<p class="time price"><span class="num">{{seller.deliveryTime}}</span>分钟</p>
					</div>
				</div>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<p class="text">{{seller.bulletin}}</p>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import star from 'components/star/star.vue';
	import split from 'components/split/split.vue';
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		components: {
			star,
			split
		},
		data() {
			return {
				isActive: false
			};
		},
		computed: {
			activeText() {
				if (this.isActive) {
					return '已收藏';
				} else {
					return '收藏';
				}
			}
		},
		created() {
			this.$nextTick(() => {
				this.scroll = new BScroll(this.$refs.seller, {
					click: true
				});
			});
		},
		methods: {
			collect() {
				this.isActive = !this.isActive;
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.seller
		position: absolute
		top: 174px
		left: 0
		bottom: 0
		width: 100%
		overflow: hidden
		.seller-content
			.overview
				padding: 0 18px
				.overview-title
					position: relative
					padding: 18px 0
					border-1px(rgba(7, 17, 27, 0.1))
					font-size: 0
					.name
						margin-bottom: 8px
						line-height: 14px
						font-size: 14px
						color: rgb(7, 17, 27)
					.star
						display: inline-block
						vertical-align: top
						margin-right: 8px
					.rank-rate, .sell-count
						display: inline-block
						vertical-align: top
						line-height: 16px
						font-size: 10px
						color: rgb(77, 83, 95)
					.rank-rate
						margin-right: 12px
					.collect
						position: absolute
						top: 18px
						right: 0
						text-align: center
						.icon-favorite
							line-height: 24px
							font-size: 24px
							color: #d4d6d9
							margin-bottom: 4px
							&.active
								color: rgb(240, 20, 20)
						.text
							line-height: 10px
							font-size: 10px
							color: rgb(77, 85, 93)
				.overview-detail
					display: flex
					padding: 18px 0
					.item
						flex: 1
						text-align: center
						border-right: 1px solid rgba(7, 17, 27, 0.1)
						&:last-child
							border-right: 0
						.text
							line-height: 10px
							font-size: 10px
							color: rgb(147, 153, 159)
							margin-bottom: 4px
						.price
							line-height: 24px
							font-size: 10px
							font-weight: 200
							color: rgb(7, 17, 27)
							.num
								font-size: 24px
			.bulletin
				padding: 18px 0 16px 0
				margin: 0 18px
				border-1px(rgba(7, 17, 27, 0.1))
				.title
					margin-bottom: 8px
					line-height: 14px
					font-size: 14px
					color: rgb(7, 17, 27)
				.text
					padding: 0 12px
					line-height: 24px
					font-size: 12px
					font-weight: 200
					color: rgb(240, 20, 20)
</style>