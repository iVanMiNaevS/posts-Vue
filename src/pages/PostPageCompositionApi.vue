<template>
	<div>
		<h1>{{ likes }}</h1>
		<button @click="addLike">+</button>
		<h1>Page with posts</h1>
		<MyInput v-focus v-model="searchQuery" placeholder="Search..." />
		<div class="app__btns">
			<MyButton @click="showDialog">Create post</MyButton>
			<MySelect v-model="selectedSort" :options="sortOptions" />
		</div>

		<MyDialog v-model:show="dialogVisible">
			<PostForm />
		</MyDialog>
		<PostsList :posts="sortedAndSearchedPosts" v-if="!isPostsLoading" />
		<div v-else>Loading</div>
	</div>
</template>

<script>
import axios from "axios";
import PostForm from "@/components/PostForm.vue";
import PostsList from "@/components/PostsList.vue";
import {ref} from "vue";
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchPosts from "@/hooks/useSortedAndSearchPosts";

export default {
	components: {PostForm, PostsList},
	data() {
		return {
			dialogVisible: false,
			sortOptions: [
				{value: "title", name: "name"},
				{value: "body", name: "description"},
			],
		};
	},
	setup(props) {
		const {posts, totalPage, isPostsLoading} = usePosts(10);
		const {sortedPosts, selectedSort} = useSortedPosts(posts);
		const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchPosts(sortedPosts);
		return {
			posts,
			totalPage,
			isPostsLoading,
			sortedPosts,
			selectedSort,
			searchQuery,
			sortedAndSearchedPosts,
		};
	},
};
</script>
<style>
.app__btns {
	display: flex;
	justify-content: space-between;
	margin: 15px 0px;
}
</style>
