<template>
	<div>
		<h1>Page with posts</h1>
		<MyInput
			v-focus
			:model-value="searchQuery"
			@update:model-value="setSearchQuery"
			placeholder="Search..."
		/>
		<div class="app__btns">
			<MyButton @click="showDialog">Create post</MyButton>
			<MySelect
				:model-value="selectedSort"
				@update:model-value="setSelectedSort"
				:options="sortOptions"
			/>
		</div>

		<MyDialog v-model:show="dialogVisible">
			<PostForm @create="createPost" />
		</MyDialog>
		<PostsList :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading" />
		<div v-else>Loading</div>
		<div v-intersection="loadMorePosts" class="observer"></div>
		<!-- <ButtonPage :page="page" :totalPage="totalPage" @updatePage="changePage" /> -->
	</div>
</template>

<script>
import axios from "axios";
import PostForm from "@/components/PostForm.vue";
import PostsList from "@/components/PostsList.vue";
import {mapState, mapActions, mapMutations, mapGetters} from "vuex";
export default {
	components: {PostForm, PostsList},
	data() {
		return {
			dialogVisible: false,
		};
	},
	methods: {
		...mapMutations({
			setPage: "post/setPage",
			setSearchQuery: "post/setSearchQuery",
			setSelectedSort: "post/setSelectedSort",
		}),
		...mapActions({
			loadMorePosts: "post/loadMorePosts",
			fetchPosts: "post/fetchPosts",
		}),
		createPost(post) {
			this.posts.push(post);
			this.dialogVisible = false;
		},
		removePost(post) {
			this.posts = this.posts.filter((p) => p.id !== post.id);
		},
		showDialog() {
			this.dialogVisible = true;
		},
	},
	mounted() {
		this.fetchPosts();
	},
	computed: {
		...mapState({
			posts: (state) => state.post.posts,
			dialogVisible: (state) => state.post.dialogVisible,
			isPostsLoading: (state) => state.post.isPostsLoading,
			selectedSort: (state) => state.post.selectedSort,
			sortOptions: (state) => state.post.sortOptions,
			page: (state) => state.post.page,
			limit: (state) => state.post.limit,
			totalPage: (state) => state.post.totalPage,
			searchQuery: (state) => state.post.searchQuery,
		}),
		...mapGetters({
			sortedPosts: "post/sortedPosts",
			sortedAndSearchedPosts: "post/sortedAndSearchedPosts",
		}),
	},
	watch: {
		// page() {
		// 	this.fetchPosts();
		// },
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
