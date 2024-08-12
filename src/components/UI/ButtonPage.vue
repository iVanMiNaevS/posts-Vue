<template>
	<div class="page__wrapper">
		<div
			class="page"
			:class="{
				'current-page': page === pageNumber,
			}"
			v-for="pageNumber in totalPage"
			:key="pageNumber"
			@click="changePage(pageNumber)"
		>
			{{ pageNumber }}
		</div>
	</div>
</template>

<script>
export default {
	name: "ButtonPage",
	props: {
		page: Number,
		totalPage: Number,
	},
	methods: {
		changePage(pageNumber) {
			this.$emit("updatePage", pageNumber);
		},
	},
};
</script>

<style scoped>
.page__wrapper {
	display: flex;
	margin-top: 15px;
}
.page {
	border: 1px solid black;
	padding: 10px;
}
.current-page {
	border: 2px solid teal;
}
</style>
