<template>
	<div class="goods">
		<div class="menu-wrapper">
			<ul>
				<li v-for="item in goods" class="menu-item">
					<span class="text border-1px">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper">
			<ul class="foods-list">
				<li v-for="item in goods" class="foods-item">
					<h1 class="title">{{item.name}}</h1>
					<ul class="food-detail">
						<li v-for="food in item.foods" class="detail border-1px">
							<div class="icon">
								<img width="64" height="64" :src="food.icon">
							</div>
							<div class="info">
								<h1 class="name">{{food.name}}</h1>
								<P class="description">{{food.description}}</P>
								<p class="sell">
									<span class="sellCount">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}</span>
								</p>
								<p class="price">
									<span class="newprice">{{food.price}}</span>
									<span v-show="food.oldprice" class="oldprice">{{food.oldprice}}</span>
								</p>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	const ERR_OK = 0;
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				goods: []
			};
		},
		created() {
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if (response.errno === ERR_OK) {
					this.goods = response.data;
				}
			});
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.goods
		display: flex
		position: absolute
		top: 174px
		bottom: 46px
		width: 100%
		overflow: hidden
		.menu-wrapper
			flex: 0 0 80px
			width: 80px
			background-color: #f3f5f7
			.menu-item
				display: table
				width: 56px
				height: 54px
				padding: 0 12px
				.text
					display: table-cell
					vertical-align: middle
					width: 56px
					height: 54px
					font-size: 12px
					font-weight: 200
					line-height: 14px
					border-1px(rgba(7, 17, 27, 0.1))
					.icon
						display: inline-block
						vertical-align: top
						width: 12px
						height: 12px
						margin: 0 2px 2px 0
						background-size: 12px 12px
						background-repeat: no-repeat
						&.decrease
							bg-image('decrease_3')
						&.discount
							bg-image('discount_3')
						&.special
							bg-image('special_3')
						&.invoice
							bg-image('invoice_3')
						&.guarantee
							bg-image('guarantee_3')
		.foods-wrapper
			flex: 1
			.foods-list
				.foods-item
					.title
						height: 26px
						font-size: 12px
						color: rgb(147, 153, 159)
						line-height: 26px
						background-color: #f3f5f7
						padding-left: 14px
						border-left: 3px solid #d9dde1
					.food-detail
						.detail
							padding: 18px
							font-size: 0
							border-1px(rgba(7, 17, 27, 0.1))
							.icon
								display: inline-block
								vertical-align: top
								width: 64px
								height: 64px
								border-radius: 2px
								margin-right: 10px
								overflow: hidden
							.info
								display: inline-block
								.name
									font-size: 14px
									line-height: 14px
									color: rgb(7, 17, 27)
									margin-bottom: 8px
								.description
									font-size: 10px
									line-height: 10px
									color: rgb(147, 153, 159)
									margin-bottom: 8px
								.sell
									font-size: 10px
									line-height: 10px
									color: rgb(147, 153, 159)
									.sellCount
										padding-right: 12px
								.price
									.newprice
										font-size: 14px
										line-height: 24px
										font-weight: 700
										color: red
										padding-right: 8px
									.oldprice
										font-size: 10px
										font-weight: normal
										color: rgb(147, 153, 159)
</style>