<template>
	<transition name="slide">
		<div class="good" v-show="showFlag" ref="food">
			<div>
				<div class="image-wrapper">
					<img :src="food.image" />
					<div class="icon" @click="hide">
						<i class="icon-arrow_lift"></i>
					</div>
				</div>
				<div class="info">
					<h1 class="title">{{food.name}}</h1>
					<div class="sell">
						<span class="sellCount">月售{{food.sellCount}}份</span><span class="sellRating">好评率{{food.rating}}%</span>
					</div>
					<div class="price">
						<span class="new">￥{{food.price}}</span>
						<span class="old" v-show="food.oldPrice>0">￥{{food.oldPrice}}</span>
					</div>
					<div class="cartcontrol-wrapper" v-show="food.count>0">
						<cartcontrol :food="food"></cartcontrol>
					</div>
					<transition name="move">
						<div class="add" v-show="food.count===0 || !food.count" @click="addFirst">加入购物车</div>
					</transition>
				</div>
				<split v-show="food.info"></split>
				<div class="intro" v-show="food.info">
					<h1 class="title">商品介绍</h1>
					<P class="text">{{food.info}}</P>
				</div>
				<split></split>
				<div class="ratings">
					<h1 class="title">商品评价</h1>
					<ratingSelect :ratings="food.ratings" :select-type="selectType" :has-content="hasContent" :desc="desc"></ratingSelect>
					<div class="rating-wrapper">
						<ul v-show="food.ratings && food.ratings.length">
							<li v-show="needShow(item.rateType,item.text)" class="rating-item" v-for="item in food.ratings">
								<div class="user">
									<span class="username">{{item.username}}</span><img class="avatar" :src="item.avatar" width="12" height="12"></img>
								</div>
								<div class="time">{{item.rateTime | formatDate}}</div>
								<p class="content">
									<i class="icon" :class="[item.rateType===1?'icon-thumb_down':'icon-thumb_up']"></i><span class="text">{{item.text}}</span>
								</p>
							</li>
						</ul>
						<div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评论</div>
					</div>
				</div>
			</div>
		</div>
	</transition>
</template>
<script>
	import Vue from 'vue';
	import BScroll from 'better-scroll';
	import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
	import ratingSelect from 'components/ratingSelect/ratingSelect.vue';
	import split from 'components/split/split.vue';
	import { eventBus } from '../../common/js/event-bus.js';
	import { formatDate } from '../../common/js/date.js';
	// const POSITION = 0;
	// const NEGATIVE = 1;
	const ALL = 2;
	export default {
		components: {
			cartcontrol,
			ratingSelect,
			split
		},
		props: {
			food: {
				type: Object
			}
		},
		data() {
			return {
				showFlag: false,
				selectType: ALL,
				hasContent: false,
				desc: {
					all: '全部',
					position: '推荐',
					negative: '吐槽'
				}

			};
		},
		methods: {
			show() {
				this.showFlag = true;
				this.selectType = ALL;
				this.hasContent = false;
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$refs.food, {
							click: true
						});
					} else {
						this.scroll.refresh();
					}
				});
			},
			hide() {
				this.showFlag = false;
			},
			addFirst(event) {
				if (!event._constructed) {
					return;
				}
				eventBus.$emit('cartAdd', event.target);
				Vue.set(this.food, 'count', 1);
			},
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
		},
		created() {
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
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.good
		position: fixed
		top: 0
		left: 0
		bottom: 48px
		width: 100%
		background-color: #fff
		z-index: 30
		overflow: hidden
		transform: translate3d(0, 0, 0)
		&.slide-enter-active, &.slide-leave-active
			transition: all 0.2s linear
		&.slide-enter, &.slide-leave-to
			transform: translate3d(-100%, 0, 0)
		&.slide-leave, &.slide-enter-to
			transform: translate3d(0, 0, 0)
		.image-wrapper
			position: relative
			width: 100%
			height: 0
			padding-top: 100%
			img
				position: absolute
				top: 0
				left: 0
				width: 100%
				height: 100%
			.icon
				position: absolute
				top: 0
				left: 0
				padding: 10px
				.icon-arrow_lift
					font-size: 14px
					color: #fff
		.info, .intro
			padding: 18px
			background-color: #fff		
		.title
			font-size: 14px
			font-weight: 700
			color: rgb(7, 17, 27)
			line-height: 14px
		.info
			position: relative
			.title
				margin-bottom: 8px
			.sell
				margin-bottom: 18px
				font-size: 10px
				color: rgb(147, 153, 159)
				line-height: 10px
				.sellCount
					margin-right: 12px
			.price
				font-size: 0
				line-height: 24px
				.new
					font-size: 14px
					font-weight: 700
					color: rgb(240, 20, 20)
					margin-right: 12px
				.old
					text-decoration: line-through
					font-weight: 700
					font-size: 10px
					color: rgb(147, 153, 159)
			.cartcontrol-wrapper
				position: absolute
				right: 12px
				bottom: 12px
			.add
				position: absolute
				right: 18px
				bottom: 18px
				height: 24px
				border-radius: 12px
				background-color: rgb(0, 160, 220)
				font-size: 10px
				line-height: 24px
				text-align: center
				color: rgb(255, 255,255)
				padding: 0 12px
				box-sizing: border-box
				z-index: 10
				opacity: 1
				&.move-enter-active, &.move-leave-active
					transition: all 0.2s
				&.move-enter, &.move-leave-to
					opacity: 0
				&.move-leave, &.move-enter-to
					opacity: 1
		.intro
			.title
				margin-bottom: 6px
			.text
				padding: 0 8px
				font-size: 12px
				font-weight: 200
				color: rgb(77, 85, 93)
				line-height: 24px
		.ratings
			background-color: #fff
			.title
				padding: 18px 0 0 18px
			.rating-wrapper
				padding: 0 18px
				.rating-item
					position: relative
					padding: 16px 0
					border-1px(rgba(7, 17, 27, 0.1))
					&:last-child
						border-none()
					.user
						position: absolute
						top: 16px
						right: 0
						.username
							display: inline-block
							vertical-align: top
							margin-right: 6px
							font-size: 10px
							line-height: 12px
							color: rgb(147, 153, 159)
						.avatar
							border-radius: 50%
					.time
						margin-bottom: 6px
						line-height: 12px
						font-size: 10px
						color: rgb(147, 153, 159)						
					.content
						font-size: 12px
						line-height: 16px
						.icon
							margin-right: 4px						
							&.icon-thumb_down
								color: rgb(147, 153, 159)
							&.icon-thumb_up
								color: rgb(0, 160, 220)
						.text
							color: rgb(7, 17, 27)
</style>