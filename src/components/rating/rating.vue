<template>
	<div class="rating">
		<div class="header">
			<span class="all">全部{{selectedFood.ratings.length}}</span>
			<span class="recommend">推荐{{recommentCount}}</span>
			<span class="complain">吐槽{{complainCount}}</span>
		</div>
		<div class="content"></div>
	</div>
</template>
<script>
	export default {
		props: {
			selectedFood: {
				type: Object
			}
		},
		computed: {
			recommendCount() {
				let count = 0;
				while (selectedFood.ratings.ratingType === 1) {
					count++;
				}
				return count;
			},
			complainCount() {
				let count = 0;
				while (selectedFood.ratings.ratingType === 0) {
					count++;
				}
				return count;
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet">
	
</style>