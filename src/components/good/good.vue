<template>
	<transition name="slide">
		<div class="good" v-show="showFlag" ref="good">
			<div class="image-wrapper">
				<img :src="food.image" />
				<div class="icon" @click="hide">
					<i class="icon-arrow_lift"></i>
				</div>
			</div>
			<div class="info">
				<h1 class="title">{{food.name}}</h1>
				<div class="sell">
					<span class="sellCount">月售{{food.sellCount}}份</span><span class="rating">好评率{food.rating}}%</span>
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
			<div class="intro" v-show="food.info">
				<h1 class="title">商品介绍</h1>
				<P class="text">{{food.info}}</P>
			</div>
			<div class="ratings">
				<h1 class="title">商品评价</h1>
			</div>
		</div>
	</transition>
</template>
<script>
	import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
	import Vue from 'vue';
	import BScroll from 'better-scroll';
	import { eventBus } from 'components/event-bus.js';
	export default {
		components: {
			cartcontrol
		},
		props: {
			food: {
				type: Object
			}
		},
		data() {
			return {
				showFlag: false
			};
		},
		methods: {
			show() {
				this.showFlag = true;
				this.BScroll = new BScroll(this.$refs.good, {
					click: true
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
			}
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
		background: #f3f5f7
		z-index: 30
		overflow: auto
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
			margin-bottom: 16px
			background-color: #fff		
			border-top: 1px solid rgba(7, 17, 27, 0.1)
			border-bottom: 1px solid rgba(7, 17, 27, 0.1)
		.title
			font-size: 14px
			font-weight: 700
			color: rgb(7, 17, 27)
			line-height: 14px
			margin-bottom: 6px
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
			.text
				padding: 0 8px
				font-size: 12px
				font-weight: 200
				color: rgb(77, 85, 93)
				line-height: 24px
		.ratings
			background-color: #fff
			border-top: 1px solid rgba(7, 17, 27, 0.1)
			.title
				padding: 18px 0 0 22px			
</style>