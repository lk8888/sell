<template>
	<div class="ratings" ref="ratings">
		<div class="ratins-content">
			<div class="overview">
				<div class="overview-left">
					<p class="score">{{seller.score}}</p>
					<p class="text">综合评分</p>
					<p class="rank">高于周边商家{{seller.rankRate}}%</p>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="text">服务态度</span>
						<div class="star-wrapper"><star :size="36" :score="seller.serviceScore"></star></div>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="text">商品评分</span>
						<div class="star-wrapper"><star :size="36" :score="seller.foodScore"></star></div>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="text">送达时间</span>
						<span class="delivery-time">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<split></split>
			<ratingSelect :ratings="ratings" :select-type="selectType" :hasContent="hasContent" ></ratingSelect>
			<div class="rating-detail">
				<ul v-show="ratings && ratings.length">
					<li class="rating-item border-1px" v-for="item in ratings" v-show="needShow(item.rateType,item.text)">
						<div class="avatar"><img :src="item.avatar" width="28" height="28" /></div>
						<div class="content">
							<div class="time">{{item.rateTime | formatDate}}</div>
							<div class="user">{{item.username}}</div>
							<div class="star-wrapper">
								<star :size="24" :score="item.score"></star>
								<span class="delivery-time" v-show="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
							</div>
							<p class="text">{{item.text}}</p>
							<div class="recommend" v-show="item.recommend && item.recommend.length">
								<i class="icon" :class="[item.rateType===1?'iconthumb_down':'icon-thumb_up']"></i>
								<span class="recommend-item" v-for="name in item.recommend">{{name}}</span>
							</div>
						</div>
					</li>
				</ul>
				<div class="no-rating" v-show="!ratings || !ratings.length">暂无评论</div>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import star from 'components/star/star.vue';
	import split from 'components/split/split.vue';
	import ratingSelect from 'components/ratingSelect/ratingSelect.vue';
	import { formatDate } from '../../common/js/date.js';
	import { eventBus } from '../../common/js/event-bus.js';
	const ERR_OK = 0;
	const ALL = 2;
	export default {
		components: {
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
						this.scroll = new BScroll(this.$refs.ratings, {
							click: true
						});
					});
				}
			});
			eventBus.$on('ratingType-select', (type) => {
				this.selectType = type;
				this.$nextTick(() => {
					this.scroll.refresh();
				});
			});
			eventBus.$on('contentToggle', (flag) => {
				this.hasContent = flag;
				this.$nextTick(() => {
					this.scroll.refresh();
				});
			});
		},
		methods: {
			needShow(type, text) {
				if (this.hasContent && !text) {
					return false;
				}
				if (this.selectType === ALL) {
					return true;
				} else {
					return type === this.selectType;
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
		bottom: 0
		width: 100%
		overflow: hidden
		.ratins-content
			.overview
				display: flex
				padding: 18px 0
				.overview-left
					flex: 0 0 138px
					padding: 6px 0
					width: 138px
					border-right: 1px solid rgba(7, 17, 27, 0.1)
					text-align: center
					@media only screen and (max-width: 320px)
						flex: 0 0 120px
						width: 120px
					.score
						margin-bottom: 6px
						line-height: 28px
						font-size: 24px
						color: rgb(255, 153, 0)
					.text
						margin-bottom: 8px
						line-height: 12px
						font-size: 12px
						color: rgb(7, 17, 27)
					.rank
						line-height: 10px
						font-size: 10px
						color: rgb(147, 153, 159)
				.overview-right
					flex: 1
					padding: 6px 0 6px 24px
					@media only screen and (max-width: 320px)
						padding-left: 6px
					.score-wrapper
						margin-bottom: 8px
						font-size: 0
						.text
							display: inline-block
							margin-right: 12px
							line-height: 18px
							vertical-align: top
							font-size: 12px
							color: rgb(7, 17, 27)
						.star-wrapper
							display: inline-block
							vertical-align: top
							margin-right: 12px
						.score
							display: inline-block
							line-height: 18px
							vertical-align: top
							font-size: 12px
							color: rgb(255, 153, 0)
					.delivery-wrapper
						font-size: 0
						.text
							margin-right: 12px
							line-height: 18px
							font-size: 12px
							color: rgb(7, 17, 27)
						.delivery-time
							line-height: 18px
							font-size: 12px
							color: rgb(147, 153, 159)
			.rating-detail
				padding: 0 18px
				.rating-item
					display: flex
					padding: 18px 0
					border-1px(rgba(7, 17, 27, 0.1))
					.avatar
						flex: 0 0 28px
						width: 28px
						verttical-align: top
						margin-right: 12px
						img
							border-radius: 50%
					.content
						position: relative
						flex: 1
						.time
							position: absolute
							top: 0
							right: 0
							font-size: 10px
							font-weight: 200
							line-height: 12px
							color: rgb(147, 153, 159)
						.user
							margin-bottom: 4px
							font-size: 10px
							line-height: 12px
							color: rgb(7, 17, 27)
						.star-wrapper
							margin-bottom: 6px
							font-size: 0
							.star
								display: inline-block
								vertical-align: top
								margin-right: 6px
							.delivery-time
								display: inline-block
								vertical-align: top
								font-size: 10px
								font-weight: 200
								line-height: 12px
								color: rgb(147, 153, 159)
						.text
							margin-bottom: 8px
							font-size: 12px
							line-height: 18px
							color: rgb(7, 17, 27)
						.recommend
							font-size: 0
							.icon
								display: inline-block
								vertical-align: top
								margin: 0 8px 4px 0
								font-size: 12px
								line-height: 16px
								&.icon-thumb_down
									color: #b7bbbf
								&.icon-thumb_up
									color: rgb(0, 160, 220)
							.recommend-item
								display: inline-block
								margin:0 8px 4px 0
								padding: 0 6px
								height: 16px
								overflow: hidden
								font-size: 9px
								line-height: 16px
								color: rgb(147, 153, 159)
								border: 1px solid rgba(7, 17, 27, 0.1)
								border-radius: 2px
								background-color: rgb(255, 255, 255)
				.no-rating
					padding: 16px 0
					font-size: 12px
					color: rgb(147, 153, 159)
</style>