<template>
	<div class="shopcart">
		<div class="content">
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
			<div class="content-right">
				<div class="pay" :class="{'highlight':totalPrice>=minPrice}">{{payDesc}}</div>
			</div>
		</div>
		<div class="ball-container">
			<div v-for="ball in balls">
				<transition name="drop">
					<div class="ball" v-show="ball.show">
						<div class="inner"></div>
					</div>
				</transition>
			</div>
		</div>
	</div>
</template>
<script>
	export default {
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
				]
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
			}
		},
		methods: {
			drop(el) {
				console.log(el);
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	.shopcart
		position: fixed
		left: 0
		bottom: 0
		height: 48px
		width: 100%
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
				transition: all 0.4s
				.inner
					width: 16px
					height: 16px
					border-radius: 50%
					background-color: rgb(0, 160, 220)
					transition: all 0.4s
</style>