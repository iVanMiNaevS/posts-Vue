<template>
	<div v-if="posts.length > 0">
		<h3>List user</h3>
		<TransitionGroup name="postList"
			><PostItem v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"
		/></TransitionGroup>
	</div>
	<h2 v-else style="color: red">List posts is empty</h2>
</template>

<script>
import PostItem from "./PostItem.vue";

export default {
	components: {
		PostItem,
	},
	props: {
		posts: {
			type: Array,
			required: true,
		},
	},
};
</script>

<style scoped>
.postList-enter-active,
.postList-leave-active {
	transition: all 0.5s ease;
}
.postList-enter-from,
.postList-leave-to {
	opacity: 0;
	transform: translateX(50px);
}
.postList-move {
	transition: transform 0.5s ease;
}
</style>
