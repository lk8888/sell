<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name"> {{seller.name}} </span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div v-if = "seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text"> {{seller.supports[0].description}} </span>
				</div>
			</div>
			<div v-if = "seller.supports" class="supports-count" @click="showDetail">
				<span class="count"> {{seller.supports.length}}个 </span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div @click="showDetail" class="bulletin-wrapper">
			<span class="bulletin-image"></span><span class="bulletin-text"> {{seller.bulletin}} </span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<div v-show="detailShow" class="detail">
			<div class="detail-title">
				<div class="name"> {{seller.name}} </div>
				<div class="appraise">
					
				</div>
			</div>
			<div class="detail-favorable"></div>
			<div class="detail-bulletin"></div>
			<div class="detail-close"></div>
		</div>
	</div>
</template>
<script>
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				detailShow: false
			};
		},
		methods: {
			showDetail() {
				this.detailShow = true;
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.header
		position: relative
		overflow: hidden
		color: rgb(255, 255, 255)
		background-color: rgba(7, 17, 27, 0.5)
		.content-wrapper
			position: relative
			padding: 24px 12px 18px 24px
			font-size: 0
			.avatar
				display: inline-block
				vertical-align: top
				img
					border-radius: 2px
			.content
				display: inline-block
				margin-left: 16px
				.title
					margin-bottom: 8px
					margin-top: 2px
					.brand
						display: inline-block
						vertical-align: top
						width: 30px
						height: 18px
						bg-image('brand')
						background-size: 30px 18px
						background-repeat: no-repeat
					.name
						margin-left: 6px
						font-size: 16px
						font-weight: bold
						line-height: 18px
				.description
					margin-bottom: 10px
					font-size: 10px
					line-height: 12px
				.supports
					margin-top: 10px
					.icon
						display: inline-block
						vertical-align: top
						width: 12px
						height: 12px
						margin: 0 4px 2px 0
						background-size: 12px 12px
						background-repeat: no-repeat
						&.decrease
							bg-image('decrease_1')
						&.discount
							bg-image('discount_1')
						&.special
							bg-image('special_1')
						&.invoice
							bg-image('invoice_1')
						&.guarantee
							bg-image('guarantee_1')
					.text
						font-size: 10px
						font-weight: 100
						line-height: 12px
			.supports-count
				display: inline-block
				position: absolute
				right: 12px
				bottom: 14px
				padding: 0 8px
				height: 24px
				line-height: 24px
				border-radius: 14px
				text-align: center
				background: rgba(0, 0, 0, 0.2)
				.count
					margin-right: 2px
					vertical-align: top
					font-size: 10px
				.icon-keyboard_arrow_right
					line-height: 24px
					font-size: 10px
		.bulletin-wrapper
			position: relative
			height: 28px
			line-height: 28px
			padding: 0 22px 0 12px
			background-color: rgba(7, 17, 27, 0.2)
			white-space: nowrap
			overflow: hidden
			text-overflow: ellipsis
			.bulletin-image
				display: inline-block
				vertical-align: top
				margin-top: 8px
				width: 22px
				height: 12px
				line-height: 28px
				font-size: 10px
				bg-image('bulletin')
				background-size: 22px 12px
				background-repeat: no-repeat
			.bulletin-text
				margin: 0 4px
				vertical-align: top
				font-size: 10px	
			.icon-keyboard_arrow_right
				position: absolute
				right: 12px
				top: 8px
				display: inline-block
				font-size: 10px
		.background
			position: absolute
			top: 0
			left: 0
			width: 100%
			height: 100%
			z-index: -1
			filter: blur(10px)
		.detail
			position: fixed
			top: 0
			left: 0
			width: 100%
			height: 100%
			z-index: 100
			overflow: auto
			padding: 64px 36px 32px 36px
			background: rgba(7, 17, 27, 0.8)
			border: 1px solid red
			.detail-title
				.name
					font-size: 16px
					font-weight: 700
					line-height: 16px
					text-align: center
					margin-bottom: 16px
				.appraise
					height: 24px
</style>