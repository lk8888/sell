<template>
	<div class="wrapper">
		<div class="shopcart">
			<div class="content" @click="toggleShow">
				<div class="content-left">
					<div class="logo-wrapper">
						<div class="logo" :class="{'highlight':totalCount>0}">
							<i class="icon icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
						</div>
						<div class="num" v-show="totalCount>0">{{totalCount}}</div>
					</div>
					<div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
					<div class="delivery">需配送费￥{{deliveryPrice}}元</div>
				</div>
				<div class="content-right" @click.stop.prevent="pay">
					<div class="pay" :class="{'highlight':totalPrice>=minPrice}">{{payDesc}}</div>
				</div>
			</div>
			<div class="ball-container">
				<div v-for="ball in balls">
					<transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
						<div class="ball" v-show="ball.show">
							<div class="inner inner-hook"></div>
						</div>
					</transition>
				</div>
			</div>
			<transition name="fold">
				<div class="shopcart-list" v-show="listShow">
					<div class="list-header">
						<h1 class="title">购物车</h1>
						<p class="empty" @click="empty">清空</p>
					</div>
					<div class="list-content" ref="listContent">
			
						<ul>
							<li class="food border-1px" v-for="food in selectFoods">
								<span class="name">{{food.name}}</span>
								<div class="pay">
									<span class="price">￥{{food.price*food.count}}</span>
									<div class="cartcontrol-wrapper">
										<cartcontrol :food="food"></cartcontrol>
									</div>
								</div>
							</li>
						</ul>
					</div>
				</div>
			</transition>
		</div>
		<transition name="fade">
			<div class="list-mask" @click="hideList" v-show="listShow"></div>
		</transition>
	</div>
</template>
<script>
	import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
	import BScroll from 'better-scroll';
	export default {
		components: {
			cartcontrol
		},
		props: {
			selectFoods: {
				type: Array,
				default() {
					return [];
				}
			},
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				balls: [
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					}
				],
				dropBalls: [],
				fold: true
			};
		},
		computed: {
			totalPrice() {
				let total = 0;
				this.selectFoods.forEach((food) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount() {
				let count = 0;
				this.selectFoods.forEach((food) => {
					count += food.count;
				});
				return count;
			},
			payDesc() {
				if (this.totalPrice === 0) {
					return `￥${this.minPrice}起送`;
				} else if (this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice;
					return `还差￥${diff}起送`;
				} else {
					return '去结算';
				}
			},
			listShow() {
				if (!this.totalCount) {
					this.fold = true;
					return false;
				}
				let show = !this.fold;
				if (show) {
					if (!this.scroll) {
						this.$nextTick(() => {
							this.scroll = new BScroll(this.$refs.listContent, {
								click: true
							});
						});
					} else {
						this.scroll.refresh();
					}
				}
				return show;
			}
		},
		methods: {
			drop(el) {
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i];
					if (!ball.show) {
						ball.show = true;
						ball.el = el;
						this.dropBalls.push(ball);
						return;
					}
				}
			},
			toggleShow() {
				if (!this.totalCount) {
					return;
				}
				this.fold = !this.fold;
			},
			hideList() {
				this.fold = true;
			},
			beforeEnter(el) {
				let count = this.balls.length;
				while (count--) {
					let ball = this.balls[count];
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect();
						let x = rect.left - 32;
						let y = -(window.innerHeight - rect.top - 22);
						el.style.display = '';
						el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
						el.style.transform = `translate3d(0, ${y}px, 0)`;
						let inner = el.getElementsByClassName('inner-hook')[0];
						inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
						inner.style.transform = `translate3d(${x}px, 0, 0)`;
					}
				}
			},
			enter(el) {
				/* eslint-disable no-unused-vars */
				let rf = el.offsetHeight;
				this.$nextTick(() => {
					el.style.webkitTransform = 'translate3d(0, 0, 0)';
					el.style.transform = 'translate3d(0, 0, 0)';
					let inner = el.getElementsByClassName('inner-hook')[0];
					inner.style.webkitTransform = 'translate3d(0, 0, 0)';
					inner.style.transform = 'translate3d(0, 0, 0)';
				});
			},
			afterEnter(el) {
				let ball = this.dropBalls.shift();
				if (ball) {
					ball.show = false;
					el.style.display = 'none';
				}
			},
			pay() {
				if (this.totalPrice < this.minPrice) {
					return;
				}
				window.alert(`总共需要支付￥${this.totalPrice + this.deliveryPrice}元`);
			},
			empty() {
				this.selectFoods.forEach((food) => {
					food.count = 0;
				});
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.shopcart
		position: fixed
		left: 0
		bottom: 0
		height: 48px
		width: 100%
		z-index: 50
		.content
			display: flex
			font-size: 0
			height: 48px
			width: 100%
			background-color: #141d27
			.content-left
				flex: 1
				.logo-wrapper
					display: inline-block
					position: relative
					top: -10px
					z-index: 50
					margin: 0 12px
					padding: 6px
					width: 56px
					height: 56px
					border-radius: 50%
					box-sizing: border-box
					background-color: #141d27
					vertical-align: top
					.logo
						width: 100%
						height: 100%
						border-radius: 50%
						background-color: #2b343c
						text-align: center
						&.highlight
							background-color: rgb(0, 160, 220)
						.icon
							font-size: 24px
							line-height: 44px
							color: #80858a
							&.highlight
								color: rgb(255, 255, 255)
					.num
						position: absolute
						top: 0
						right: 0
						width: 24px
						height: 16px
						border-radius: 8px
						font-size: 9px
						font-weight: 700
						color: rgb(255, 255, 255)
						line-height: 16px
						text-align: center
						background-color: rgb(240, 20, 20)
						box-shadow: 0px 4px 8px 0px rgb(0, 0, 0, 0.4)
				.price
					display: inline-block
					vertical-align: top
					font-size: 16px
					margin: 12px 0
					color: rgba(255, 255, 255, 0.4)
					font-weight: 700
					line-height: 24px
					padding-right: 12px
					box-sizing: border-box
					border-right: 1px solid rgba(255, 255, 255, 0.4)
					&.highlight
						color: rgb(255, 255, 255)
				.delivery
					display: inline-block
					vertical-align: top
					margin: 12px 0 0 12px
					line-height: 24px
					font-size: 10px
					color: rgba(255, 255, 255, 0.4)
			.content-right
				flex: 0 0 105px
				width: 105px
				.pay
					height: 48px
					line-height: 48px
					background-color: #2b333b
					text-align: center
					font-size: 12px
					color: rgba(255, 255, 255, 0.4)
					font-weight: 700
					&.highlight
						color: rgb(255, 255, 255)
						background-color: #00b43c 
		.ball-container
			.ball
				position: fixed
				left: 32px
				bottom: 22px
				z-index: 200
				transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
				.inner
					width: 16px
					height: 16px
					border-radius: 50%  
					background-color: rgb(0, 160, 220)
					transition: all 0.4s linear
		.shopcart-list
			position: absolute
			top: 0
			left: 0
			width: 100%
			z-index: -1
			transition: all 0.5s ease
			transform: translate3d(0, -100%, 0)
			&.fold-enter-active, &.fold-leave-active
				transition: all 0.5s ease
			&.fold-enter, &.fold-leave-to
				transform: translate3d(0, 0, 0)
			&.fold-leave, &.fold-enter-to
				transform: translate3d(0, -100%, 0)
			.list-header
				height: 40px
				line-height: 40px
				padding: 0 18px
				background-color: #f3f5f7
				border-bottom: 2px solid rgba(7, 17, 27, 0.1)
				.title
					float: left
					font-size: 14px
					font-weight: 200
					color: rgb(7, 17, 27)
				.empty
					float: right
					font-size: 12px
					color: rgb(0, 160, 220)
			.list-content
				padding: 0 18px
				max-height: 217px
				overflow: hidden
				background-color: #fff
				.food
					position: relative
					padding: 12px 0
					box-sizing: border-box
					border-1px(rgba(7, 17, 27, 0.1))
					.name
						font-size: 14px
						line-height: 24px
						color: rgb(7, 17, 27)
					.price
						position: absolute
						right: 90px
						bottom: 12px
						line-height: 24px
						font-size: 14px
						font-weight: 700
						color: rgb(240, 20, 20)
					.cartcontrol-wrapper
						position: absolute
						right: 0
						bottom: 6px
	.fade-enter-active, .fade-leave-active
		transition: all 0.5s
	.fade-enter, .fade-leave
		opacity: 0
		background-color: rgba(7, 17, 27, 0)
	.list-mask
		position: fixed
		top: 0
		left: 0
		width: 100%
		height: 100%
		z-index: 40
		background-color: rgba(7, 17, 27, 0.6)
		filter: blur(10px)
		backdrop-filter: blur(10px)
</style>