<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo">
						<i class="icon icon-shopping_cart"></i>
					</div>
				</div>
				<div class="price">￥0</div>
				<div class="delivery">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay">￥{{minPrice}}元起送</div>
			</div>
		</div>
	</div>
</template>
<script>
	export default {
		props: {
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
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
			.content-left
				flex: 1
				background-color: #141d27
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
						width: 44px
						height: 44px
						border-radius: 50%
						background-color: #2b343c
						text-align: center
						.icon
							font-size: 24px
							line-height: 44px
							color: #80858a
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
</style>