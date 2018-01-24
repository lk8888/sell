<template>
	<div class="rating">
		<div class="main-wrapper">
			<div class="main border-1px">
				<p class="all">全部
					<span class="count">{{food.ratings.length}}</span>
				</p>
				<p class="recommend">推荐
					<span class="count">{{recommendCount}}</span>
				</p>
				<p class="complain">吐槽
					<span class="count">{{complainCount}}</span>
				</p>
			</div>
		</div>
		<div class="content">
			<i class="icon-check_circle"></i><span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>
<script>
	export default {
		props: {
			food: {
				type: Object
			}
		},
		computed: {
			recommendCount() {
				let count = 0;
				let len = this.food.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.food.ratings[i].rateType === 0) {
						count++;
					}
				}
				return count;
			},
			complainCount() {
				let count = 0;
				let len = this.food.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.food.ratings[i].rateType === 1) {
						count++;
					}
				}
				return count;
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet">
	@import '../../common/stylus/mixin.styl'
	.rating
		border-bottom: 2px solid rgba(7, 17, 27, 0.1)
		.main-wrapper
			padding: 18px 18px 0 18px
			.main
				font-size: 0
				padding-bottom: 18px
				border-1px(rgba(7, 17, 27, 0.1))
				p
					display: inline-block
					padding: 8px 12px
					font-size: 12px
					line-height: 16px
					border-radius: 2px
					.count
						font-size: 8px
				.all
					color: rgb(255, 255, 255)
					background-color: rgb(0, 160, 220)
					margin-right: 8px
				.recommend
					color: rgb(77, 85, 93)
					background-color: rgba(0, 160, 220, 0.2)
					margin-right: 8px
				.complain
					color: rgb(77, 85, 93)
					background-color: rgba(77, 85, 93, 0.2)
		.content
			padding: 12px 18px
			.icon-check_circle
				font-size: 24px
				line-height: 24px
				color: rgb(147, 153, 159)
				padding-right: 4px
			.text
				display: inline-block
				vertical-align: top
				font-size: 12px
				line-height: 24px
				color: rgb(147, 153, 159)
</style>