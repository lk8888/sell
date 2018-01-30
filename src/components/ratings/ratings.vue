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
			<ratingSelect :ratings="ratings" :select-type="selectType" :hasContent="hasContent" ></ratingSelect>
			<div class="rating-detail">
				<ul>
					<li class="rating-item border-1px" v-for="item in ratings">
						<div class="avatar"><img :src="item.avatar" width="28" height="28" /></div>
						<div class="time">{{item.rateTime | formatDate}}</div>
						<div class="content">
							<div class="user">{{item.username}}</div>
							<div class="info">
								<div class="star-wrapper">
									<star :size="24" :score="item.score"></star>
								</div>
								<span class="delivery-time" v-show="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
							</div>
							<p class="text">{{item.text}}</p>
							<div class="type" v-show="item.recommend.length>0">
								<i class="icon" :class="[item.rateType===1?'icon-thumb_down':'icon-thumb_up']"></i>
								<ul>
									<li class="recommend" v-for="name in item.recommend">{{name}}</li>
								</ul>	
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
		<shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import shopcart from 'components/shopcart/shopcart.vue';
	import star from 'components/star/star.vue';
	import split from 'components/split/split.vue';
	import ratingSelect from 'components/ratingSelect/ratingSelect.vue';
	import { formatDate } from '../../common/js/date.js';
	const ERR_OK = 0;
	const ALL = 2;
	export default {
		components: {
			shopcart,
			star,
			split,
			ratingSelect
		},
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				ratings: [],
				selectType: ALL,
				hasContent: false
			};
		},
		created() {
			this.$http.get('/api/ratings').then((response) => {
				response = response.body;
				this.selectType = ALL;
				this.hasContent = false;
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
		},
		filters: {
			formatDate(time) {
				let date = new Date(time);
				return formatDate(date, 'yyyy-MM-dd hh:mm');
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl';
	.ratings
		position: absolute
		top: 174px
		left: 0
		bottom: 48px
		width: 100%
		overflow: hidden
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
			.rating-detail
				margin: 0 18px
				.rating-item
					position: relative
					padding: 18px 0
					border-1px(rgba(7, 17, 27, 0.1))
					.avatar
						position: absolute
						top: 18px
						left: 0
						verttical-align: top
						img
							border-radius: 50%
					.time
						position: absolute
						top: 18px
						right: 0
						font-size: 10px
						font-weight: 200
						line-height: 12px
						color: rgb(147, 153, 159)
					.content
						margin-left: 40px
						.user
							margin-bottom: 4px
							font-size: 10px
							line-height: 12px
							color: rgb(7, 17, 27)
						.info
							margin-bottom: 6px
							font-size: 0
							.star-wrapper
								display: inline-block
								margin-right: 6px
							.delivery-time
								font-size: 10px
								font-weight: 200
								line-height: 12px
								color: rgb(147, 153, 159)
						.text
							margin-bottom: 8px
							font-size: 12px
							line-height: 18px
							color: rgb(7, 17, 27)
						.type
							font-size: 0
							.icon
								display: inline-block
								vertical-align: top
								margin-right: 8px
								font-size: 12px
								line-height: 16px
								&.icon-thumb_down
									color: #b7bbbf
								&.icon-thumb_up
									color: rgb(0, 160, 220)
							ul
								display: inline-block
								font-size: 0
								.recommend
									display: inline-block
									margin-right: 8px
									padding: 0 6px
									font-size: 9px
									line-height: 16px
									color: rgb(147, 153, 159)
									border: 1px solid rgba(7, 17, 27, 0.1)
									border-radius: 2px
									background-color: rgb(255, 255, 255)
</style>