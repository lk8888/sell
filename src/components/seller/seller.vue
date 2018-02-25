<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<div class="overview-title">
					<h1 class="name">{{seller.name}}</h1>
					<div class="desc border-1px">
						<star :size="36" :score="seller.score"></star>
						<span class="rating-count">({{seller.rankRate}})</span>
						<span class="sell-count">月售{{seller.sellCount}}单</span>
					</div>
					<div class="collect" @click="toggleFavorite">
						<i class="icon-favorite" :class="{'active': favorite}"></i>
						<p class="text">{{favoriteText}}</p>
					</div>
				</div>
				<ul class="remark">
					<li class="block">
						<p class="title">起送价</p>
						<p class="text"><span class="num">{{seller.minPrice}}</span>元</p>
					</li>
					<li class="block">
						<p class="title">商家配送</p>
						<p class="text"><span class="num">{{seller.deliveryPrice}}</span>元</p>
					</li>
					<li class="block">
						<p class="title">平均配送时间</p>
						<p class=" text"><span class="num">{{seller.deliveryTime}}</span>分钟</p>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="activity">
				<div class="bulletin border-1px">
					<h1 class="title">公告与活动</h1>
					<p class="text">{{seller.bulletin}}</p>
				</div>
				<ul>
					<li v-for="support in seller.supports" class="support border-1px">
						<i class="icon" :class="classMap[support.type]"></i>
						<span class="des">{{support.description}}</span>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="scenery">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picWrapper">
					<ul ref="picContent">
						<li class="pic-item pic-list-hook" v-for="pic in seller.pics" ref="picItem"><img :src="pic" width="120" height="90" /></li>
					</ul>
				</div>
			</div>
			<split></split>
			<div class="infos">
				<h1 class="title border-1px">商家信息</h1>
				<ul>
					<li class="info border-1px" v-for="info in seller.infos">{{info}}</li>
				</ul>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import star from 'components/star/star.vue';
	import split from 'components/split/split.vue';
	import { saveToLocal, loadFromLocal } from '../../common/js/store.js';
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		components: {
			star,
			split
		},
		data() {
			return {
				favorite: (() => {
					return loadFromLocal(this.seller.id, 'favorite', false);
				})()
			};
		},
		computed: {
			favoriteText() {
				return this.favorite ? '已收藏' : '收藏';
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		watch: {
			'seller'() {
				this._initScroll();
				this._initPics();
			}
		},
		mounted() {
			this._initScroll();
			this._initPics();
		},
		methods: {
			toggleFavorite(event) {
				if (!event._constructed) {
					return;
				}
				this.favorite = !this.favorite;
				saveToLocal(this.seller.id, 'favorite', this.favorite);
			},
			_initScroll() {
				if (!this.scroll) {
					this.scroll = new BScroll(this.$refs.seller, {
						click: true
					});
				} else {
					this.scroll.refresh();
				}
			},
			/* _initPicScroll() {
				if (this.seller.pics) {
					let width = 0;
					for (let i = 0; i < this.seller.pics.length; i++) {
						width += this.$refs.picItem[i].getBoundingClientRect().width;// getBoundingClientRect() 返回元素的大小及其相对于视口的位置
					}
					console.log(this.seller.pics.length);
					this.$refs.picContent.style.width = width + 'px';
					this.$nextTick(() => {
						if (!this.picScroll) {
							this.picScroll = new BScroll(this.$refs.picWrapper, {
							startX: 0,
							click: true,
							scrollX: true,
							scrollY: false,
							eventPassthrough: 'vertical'
							});
						} else {
							this.picScroll.refresh();
						}
					});
				}
			} */
			_initPics() {
				if (this.seller.pics) {
					let picWidth = 120;
					let margin = 6;
					let width = (picWidth + margin) * this.seller.pics.length - margin;
					this.$refs.picContent.style.width = width + 'px';
					this.$nextTick(() => {
						if (!this.picScroll) {
							this.picScroll = new BScroll(this.$refs.picWrapper, {
								scrollX: true,
								eventPassthrough: 'vertical'
							});
						} else {
							this.picScroll.refresh();
						}
					});
				}
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.seller
		position: absolute
		top: 174px
		left: 0
		bottom: 0
		width: 100%
		overflow: hidden
		.seller-content
			.overview
				padding: 18px 0
				.overview-title
					position: relative
					padding: 0 18px
					.name
						margin-bottom: 8px
						line-height: 14px
						font-size: 14px
						color: rgb(7, 17, 27)
					.desc
						padding-bottom: 18px
						border-1px(rgba(7, 17, 27, 0.1))
						font-size: 0
						.star
							display: inline-block
							vertical-align: top
							margin-right: 8px
						.rating-count, .sell-count
							display: inline-block
							vertical-align: top
							line-height: 16px
							font-size: 10px
							color: rgb(77, 83, 95)
						.rating-count
							margin-right: 12px
					.collect
						position: absolute
						right: 11px
						top: 0
						width: 50px
						text-align: center
						.icon-favorite
							margin-bottom: 4px
							line-height: 24px
							font-size: 24px
							color: #d4d6d9
							&.active
								color: rgb(240, 20, 20)
						.text
							line-height: 10px
							font-size: 10px
							color: rgb(77, 85, 93)
				.remark
					display: flex
					padding: 18px 0 0 0
					.block
						flex: 1
						text-align: center
						border-right: 1px solid rgba(7, 17, 27, 0.1)
						&:last-child
							border-right: 0
						.title
							margin-bottom: 4px
							line-height: 10px
							font-size: 10px
							color: rgb(147, 153, 159)
						.text
							line-height: 24px
							font-size: 10px
							font-weight: 200
							color: rgb(7, 17, 27)
							.num
								font-size: 24px
			.activity
				padding: 0 18px
				.bulletin
					padding: 18px 0 16px 0
					border-1px(rgba(7, 17, 27, 0.1))
					.title
						margin-bottom: 8px
						line-height: 14px
						font-size: 14px
						color: rgb(7, 17, 27)
					.text
						padding: 0 12px
						line-height: 24px
						font-size: 12px
						font-weight: 200
						color: rgb(240, 20, 20)
				.support
					padding: 16px 12px
					border-1px(rgba(7, 17, 27, 0.1))
					font-size: 0
					&:last-child
						border-none()
					.icon
						display: inline-block
						vertical-align: top
						margin-right: 6px
						width: 16px
						height: 16px
						background-size: 16px 16px
						background-repeat: no-repeat
						&.decrease
							bg-image('decrease_4')
						&.discount
							bg-image('discount_4')
						&.special
							bg-image('special_4')
						&.invoice
							bg-image('invoice_4')
						&.guarantee
							bg-image('guarantee_4')
					.des
						display: inline-block
						vertical-align: top
						line-height: 16px
						font-size: 12px
						fonot-weight: 200
						color: rgb(7, 17, 27)
			.scenery
				padding: 18px 0 18px 18px
				.title
					margin-bottom: 12px
					line-height: 14px
					font-size: 14px
					color: rgb(7, 17, 27)
				.pic-wrapper
					overflow: hidden
					white-space: nowrap
					text-overflow: ellipsis
					width: 100%
					ul
						font-size: 0
						.pic-item
							display: inline-block
							margin-right: 6px
							width: 120px
							height: 90px
							&:last-child
								margin-right: 0
			.infos
				padding: 0 18px
				.title
					padding: 18px 0 12px 0
					line-height: 14px
					font-size: 14px
					color: rgb(7, 17, 27)
					border-1px(rgba(7, 17, 27, 0.1))
				.info
					padding: 16px 12px
					line-height: 16px
					font-size: 12px
					font-weight: 200
					color: rgb(7, 17, 27)
					border-1px(rgba(7, 17, 27, 0.1))
					&:last-child
						border-none()
</style>