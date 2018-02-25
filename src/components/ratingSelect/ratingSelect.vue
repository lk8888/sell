<template>
	<div class="ratingSelect">
		<div class="select-type border-1px">
			<p class="block positive" :class="{'active':changeType===2}" @click="selected(2,$event)">
				<span class="text">{{desc.all}}</span><span class="count">{{ratings.length}}</span>
			</p>
			<p class="block positive" :class="{'active':changeType===0}" @click="selected(0,$event)">
				<span class="text">{{desc.positive}}</span><span class="count">{{positives.length}}</span>
			</p>
			<p class="block negative" :class="{'active':changeType===1}" @click="selected(1,$event)">
				<span class="text">{{desc.negative}}</span><span class="count">{{negatives.length}}</span>
			</p>
		</div>
		<div class="content">
			<i class="icon-check_circle" :class="{'active':toggleFlag===true}" @click="toggleContent"></i><span class="text" @click="toggleContent">只看有内容的评价</span>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import { eventBus } from '../../common/js/event-bus.js';
	const POSITIVE = 0;
	const NEGATIVE = 1;
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
		data() {
			return {
				changeType: this.selectType,
				toggleFlag: this.hasContent
			};
		},
		computed: {
			positives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === POSITIVE;
				});
			},
			negatives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === NEGATIVE;
				});
			}
		},
		methods: {
			selected(type, event) {
				if (!event._constructed) {
					return;
				}
				this.changeType = type;
				eventBus.$emit('ratingType-select', type);
			},
			toggleContent() {
				if (!event._constructed) {
					return;
				}
				this.toggleFlag = !this.toggleFlag;
				eventBus.$emit('contentToggle', this.toggleFlag);
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
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
				&.active
					color: #00c850
			.text
				display: inline-block
				vertical-align: top
				font-size: 12px
				line-height: 24px
				color: rgb(147, 153, 159)
</style>