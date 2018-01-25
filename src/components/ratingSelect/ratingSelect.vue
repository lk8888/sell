<template>
	<div class="ratingSelect">
		<div class="select-type border-1px">
			<p class="block positive" :class="{'active':selectType===2}">
				<span class="text">{{desc.all}}</span><span class="count">{{ratings.length}}</span>
			</p>
			<p class="block positive" :class="{'active':selectType===0}">
				<span class="text">{{desc.position}}</span><span class="count">{{recommendCount}}</span>
			</p>
			<p class="block negative" :class="{'active':selectType===1}">
				<span class="text">{{desc.negative}}</span><span class="count">{{complainCount}}</span>
			</p>
		</div>
		<div class="content">
			<i class="icon-check_circle"></i><span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>
<script>
	// const POSITIVE = 0;
	// const NEGATIVE = 1;
	const ALL = 2;
	export default {
		props: {
			ratings: {
				type: Array,
				default() {
					return [];
				}
			},
			selectType: {
				type: Number,
				default: ALL
			},
			hasContent: {
				type: Boolean,
				default: false
			},
			desc: {
				type: Object,
				default() {
					return {
						all: '全部',
						positive: '满意',
						negative: '不满意'
					};
				}
			}
		},
		computed: {
			recommendCount() {
				let count = 0;
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.ratings[i].rateType === 1) {
						count++;
					}
				}
				return count;
			},
			complainCount() {
				let count = 0;
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.ratings[i].rateType === 0) {
						count++;
					}
				}
				return count;
			}
		},
		methods: {
			all() {
				this.selectType = [];
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					this.selectType.push(this.ratings[i]);
				}
				this.$emit('transfer', this.selectType);
			},
			recommend() {
				this.selectType = [];
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.ratings[i].rateType === 1) {
						this.selectType.push(this.ratings[i]);
					}
				}
				this.$emit('transfer', this.selectType);
			},
			complain() {
				this.selectType = [];
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.ratings[i].rateType === 0) {
						this.selectType.push(this.ratings[i]);
					}
				}
				this.$emit('transfer', this.selectType);
			},
			hasText() {
				this.selectType = [];
				let len = this.ratings.length;
				for (let i = 0; i < len; i++) {
					if (this.ratings[i].text) {
						this.selectType.push(this.ratings[i]);
					}
				}
				this.$emit('transfer', this.selectType);
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet">
	@import '../../common/stylus/mixin.styl'
	.ratingSelect
		border-bottom: 2px solid rgba(7, 17, 27, 0.1)
		.select-type
			padding: 18px 0
			margin: 0 18px
			font-size: 0
			border-1px(rgba(7, 17, 27, 0.1))
			.block
				display: inline-block
				padding: 8px 12px
				font-size: 12px
				line-height: 16px
				border-radius: 2px
				color: rgb(77, 85, 93)
				&.active
					color: rgb(255, 255, 255)
				.count
					font-size: 8px
					margin-left: 2px
				&.positive
					background-color: rgba(0, 160, 220, 0.2)
					margin-right: 8px
					&.active
						background-color: rgb(0, 160, 220)
				&.negative
					background-color: rgba(77, 85, 93, 0.2)
					&.active
						background-color: rgb(77, 85, 93)
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