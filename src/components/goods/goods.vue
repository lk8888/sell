<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item menu-list-hook" :class="{'current':currentIndex===index}" @click="selectMenu(index, $event)">
					<span class="text border-1px">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" class="foods-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item border-1px">
							<div class="icon">
								<img width="57" height="57" :src="food.icon">
							</div>
							<div class="info">
								<h1 class="name">{{food.name}}</h1>
								<P class="description">{{food.description}}</P>
								<p class="sell">
									<span class="sellCount">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
								</p>
								<p class="price">
									<span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
								</p>
								<div class="cartcontrol-wrapper">
									<cartcontrol @cart="_drop" :food="food"></cartcontrol>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
</template>
<script>
	import shopcart from 'components/shopcart/shopcart.vue';
	import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
	import BScroll from 'better-scroll';
	const ERR_OK = 0;
	export default {
		components: {
			shopcart,
			cartcontrol
		},
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				goods: [],
				listHeight: [],
				scrollY: 0
			};
		},
		computed: {
			currentIndex() {
				let menuList = this.$refs.menuWrapper.getElementsByClassName('menu-list-hook');
				for (let i = 0; i < this.listHeight.length; i++) {
					let height1 = this.listHeight[i];
					let height2 = this.listHeight[i + 1];
					if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
						let el = menuList[i];
						this.menuScroll.scrollToElement(el, 300);
						return i;
					}
				}
				return 0;
			},
			selectFoods() {
				let foods = [];
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if (food.count) {
							foods.push(food);
						}
					});
				});
				return foods;
			}
		},
		created() {
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if (response.errno === ERR_OK) {
					this.goods = response.data;
					this.$nextTick(() => {
						this._initScroll();
						this._calculateHeight();
					});
				}
			});
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		methods: {
			selectMenu(index, event) {
				if (!event._constructed) {
					return;
				}
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let el = foodList[index];
				this.foodsScroll.scrollToElement(el, 300);
			},
			_initScroll() {
				this.menuScroll = new BScroll(this.$refs.menuWrapper, {
					click: true
				});
				this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
					click: true,
					probeType: 3
				});
				this.foodsScroll.on('scroll', (pos) => {
					this.scrollY = Math.abs(Math.round(pos.y));
				});
			},
			_calculateHeight() {
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let height = 0;
				this.listHeight.push(height);
				for (let i = 0; i < foodList.length; i++) {
					let item = foodList[i];
					height += item.clientHeight;
					this.listHeight.push(height);
				}
			},
			_drop(target) {
				// 体验优化，执行异步下落动画
				this.$nextTick(() => {
					this.$refs.shopcart.drop(target);
				});
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	@import '../../common/stylus/base.styl'
	.goods
		display: flex
		position: absolute
		top: 174px
		bottom: 48px
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
				line-height: 14px
				&.current
					position: relative
					margin-top: -1px
					z-index: 10
					background-color: #fff
					.text
						border-none()
						font-weight: 400	
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
				.title
					height: 26px
					font-size: 12px
					color: rgb(147, 153, 159)
					line-height: 26px
					background-color: #f3f5f7
					padding-left: 14px
					border-left: 2px solid #d9dde1
				.food-item
					display: flex
					margin: 18px
					padding-bottom: 18px
					font-size: 0
					border-1px(rgba(7, 17, 27, 0.1))
					&:last-child
						border-none()
						margin-bottom: 0
					.icon
						flex: 0 0 57px
						vertical-align: top
						width: 57px
						height: 57px
						border-radius: 2px
						margin-right: 10px
						overflow: hidden
					.info
						flex: 1
						font-size: 0
						.name
							font-size: 14px
							line-height: 14px
							color: rgb(7, 17, 27)
							margin: 2px 0 8px 0
						.description
							font-size: 10px
							line-height: 12px
							color: rgb(147, 153, 159)
							margin-bottom: 8px
						.sell
							font-size: 10px
							line-height: 10px
							color: rgb(147, 153, 159)
							.sellCount
								padding-right: 12px
						.price
							.now
								font-size: 14px
								line-height: 24px
								font-weight: 700
								color: rgb(240, 20, 20)
								padding-right: 8px
							.old
								text-decoration: line-through
								font-size: 10px
								font-weight: normal
								color: rgb(147, 153, 159)
						.cartcontrol-wrapper
							position: absolute
							right: 0
							bottom: 0

</style>