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
		<transition name="fade">
			<div v-show="detailShow" class="detail">
				<div class="detail-wrapper clearfix">
					<div class="detail-content">
						<h1 class="name"> {{seller.name}} </h1>
						<div class="star"></div>
						<div class="title">
							<div class="line"></div>
							<div class="text">优惠信息</div>
							<div class="line"></div>
						</div>
						<ul class="supports" v-if="seller.supports">
							<li v-for="item in seller.supports" class="supports-item">
								<span class="icon":class="classMap[item.type]"></span>
								<span class="text"> {{item.description}} </span>
							</li>
						</ul>
						<div class="title">
							<div class="line"></div>
							<div class="text">商家公告</div>
							<div class="line"></div>
						</div>
						<div class="bulletin">
							<p class="text">{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
				<div class="detail-close" @click="hideDetail">
					<i class="icon-close"></i>
				</div>
			</div>
		</transition>
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
			},
			hideDetail() {
			this.detailShow = false;
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	@import '../../common/stylus/base.styl'
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
						line-height: 20px
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
		.fade-enter-active, .fade-leave-active
			transition:	all .5s
		.fade-enter, .fade-leave-to
			opacity: 0
			background: rgba(7, 17, 27, 0)
		.fade-leave, .fade-enter-to
			opacity: 1
			background: rgba(7, 17, 27, 0.8)
		.detail
			position: fixed
			top: 0
			left: 0
			width: 100%
			height: 100%
			z-index: 100
			overflow: auto
			background: rgba(7, 17, 27, 0.8)
			.detail-wrapper
				height: auto
				min-height: 100%
				width: 100%
				.detail-content	
					margin-top: 64px
					padding-bottom: 64px
					h1
						font-size: 16px
						font-weight: 700
						line-height: 16px
						text-align: center
					.star
						margin: 16px auto 0 auto
						height: 24px
					.title
						display: flex
						width: 80%
						margin: 28px auto 24px auto
						.line
							flex: 1
							position: relative
							top: -6px
							border-bottom: 1px solid rgba(255, 255, 255, 0.2)
						.text
							font-size: 14px
							line-height: 14px
							font-weight: 700
							margin: 0 12px
					.supports
						width: 80%
						margin: 0 auto
						.supports-item
							padding: 0 12px
							margin-bottom: 12px
							font-size: 0
							&: last-child
								margin-bottom: 0
							.icon
								display: inline-block
								vertical-align: top
								width: 16px
								height: 16px
								margin-right: 6px
								background-size: 16px 16px
								background-repeat: no-repeat
								&.decrease
									bg-image('decrease_2')
								&.discount
									bg-image('discount_2')
								&.special
									bg-image('special_2')
								&.invoice
									bg-image('invoice_2')
								&.guarantee
									bg-image('guarantee_2')
							.text
								font-size: 12px
								font-weight: 200
								line-height: 16px
					.bulletin
						width: 80%
						margin: 0 auto
						.text
							padding: 0 12px
							font-size: 12px
							line-height: 24px
			.detail-close
				position: relative
				margin: -64px auto 0 auto
				width: 32px
				height: 32px
				line-height: 32px
				font-size: 32px
				color: rgba(255, 255, 255, 0.5)
				clear: both

</style>