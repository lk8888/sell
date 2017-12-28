<template>
	<div class="star" :class="starType">
		<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
	</div>	
</template>
<script>
	export default {
		props: {
			size: {
				type: Number
			},
			score: {
				type: Number
			}
		},
		computed: {
			starType() {
				return starType = 'star-' + this.size;
			}
		}
	}
</script>
<style rel="stylesheet/stylus" lang="stylus">
	.star
		font-size: 0
			.star-item
				display: inline-block
				background: no-repeat
		&.star-48
			.star-item
				width: 20px
				height: 20px
				margin-right: 22px
				background-size: 20px 20px
				background-repeat: no-repeat
				&:last-child
					margin-right: 0
				&.on
					bg-image("star48_on")
				&.half
					bg-image("star48_half")
				&.off
					gb-image("star48_off")
		&.star-36
		&.star-24
</style>