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
					<span class="icon"></span>
					<span class="text"> {{seller.supports[0].description}} </span>
				</div>
				<div v-if = "seller.supports" class="more">
					<span> {{seller.supports.length}} </span>
					<span>&gt;</span>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper"></div>
	</div>
</template>
<script>
	export default {
		props: {
			seller: {
				type: Object
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.header
		width: 100%
		height: auto
		color: rgb(255, 255, 255)
		background: #000
		.content-wrapper
			width: 100%
			height: auto
			padding: 24px 12px 18px 24px
			font-size: 0
			.avatar
				display: inline-block
				vertical-align: top
				margin-right: 16px
				img
					border-radius: 2px
			.content
				display: inline-block
				width: auto
				height: auto
				position: relative
				.title
					width: auto
					height: auto
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
					font-weight: 100
					line-height: 12px
				.supports
					margin-top: 10px
					.icon
						display: inline-block
						vertical-align: top
						width: 12px
						height: 12px
						margin: 0 4px 2px 0
						bg-image('decrease_1')
						background-size: 12px 12px
						background-repeat: no-repeat
					.text
						font-size: 10px
						font-weight: 100
						line-height: 12px

			.more
				float: right
				width: 40px
				height: 24px
				background-color: red
				border-radius: 8px
				right: 0
				bottom: 0
				text-align: center
				& < span
					display: inline-block
					width: 100
					hieght: auto
					font-size: 10px
					color: grey
					font-weight: 100
					line-height: 12px
				



</style>