<template>
	<div class="ratings" ref="ratings">
		<div class="ratins-content">
			<div class="score">
				<div class="score-average">
					<p class="score-total">{{seller.score}}</p>
					<p class="score-text">综合评分</p>
					<p class="rank-rate">高于周边商家{{seller.rankRate}}%</p>
				</div>
				<div class="score-detail">
					<div class="server">
						<span class="text">服务态度</span>
						<div class="star-wrapper"><star :size="36" :score="seller.serviceScore"></star></div>
						<span class="service-score">{{seller.serviceScore}}</span>
					</div>
					<div class="server">
						<span class="text">服务态度</span>
						<div class="star-wrapper"><star :size="36" :score="seller.foodScore"></star></div>
						<span class="service-score">{{seller.foodScore}}</span>
					</div>
					<div class="server">
						<span class="text">送达时间</span>
						<span class="delivery-time">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<split></split>
			<shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import shopcart from 'components/shopcart/shopcart.vue';
	import star from 'components/star/star.vue';
	import split from 'components/split/split.vue';
	const ERR_OK = 0;
	export default {
		components: {
			shopcart,
			star,
			split
		},
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				ratings: {
					type: Object
				}
			};
		},
		create() {
			this.$http.get('/api/ratings').then((response) => {
				response = response.body;
				if (response.errno === ERR_OK) {
					this.ratings = response.data;
					this.$nextTick(() => {
						this._initScroll();
					});
				}
			});
		},
		methods: {
			_initScroll() {
				if (!this.scroll) {
					this.scroll = new BScroll(this.$refs.ratings, {
						click: true
					});
				} else {
					this.scroll.refresh();
				}
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	.ratings
		position: absolute
		top: 174px
		left: 0
		bottom: 48px
		width: 100%
		.ratins-content
			.score
				display: flex
				padding: 18px 0
				.score-average
					flex: 0 0 138px
					width: 138px
					border-right: 1px solid rgba(7, 17, 27, 0.1)
					text-align: center
					.score-total
						margin-bottom: 6px
						line-height: 28px
						font-size: 24px
						color: rgb(255, 153, 0)
					.score-text
						margin-bottom: 8px
						line-height: 12px
						font-size: 12px
						color: rgb(7, 17, 27)
					.rank-rate
						margin-bottom: 6px
						line-height: 10px
						font-size: 10px
						color: rgb(147, 153, 159)
				.score-detail
					flex: 1
					padding: 0 24px
					.server
						margin-bottom: 8px
						font-size: 0
						&:last-child
							margin-bottom: 0
						.text
							display: inline-block
							margin-right: 12px
							font-size: 12px
							line-height: 18px
							color: rgb(7, 17, 27)
						.star-wrapper
							display: inline-block
							vertical-align: top
							margin-right: 12px
						.service-score
							display: inline-block
							font-size: 12px
							line-height: 18px
							color: rgb(255, 153, 0)
						.delivery-time
							font-size: 12px
							line-height: 18px
							color: rgb(147, 153, 159)


</style>