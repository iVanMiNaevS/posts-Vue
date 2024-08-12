<template>
	<div>
		<h1>Page with posts</h1>
		<MyInput v-focus v-model="searchQuery" placeholder="Search..." />
		<div class="app__btns">
			<MyButton @click="showDialog">Create post</MyButton>
			<MySelect v-model="selectedSort" :options="sortOptions" />
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

export default {
	components: {PostForm, PostsList},
	data() {
		return {
			posts: [],
			dialogVisible: false,
			isPostsLoading: false,
			selectedSort: "",
			sortOptions: [
				{value: "title", name: "name"},
				{value: "body", name: "description"},
			],
			page: 1,
			limit: 10,
			totalPage: 1,
			searchQuery: "",
		};
	},
	methods: {
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
		// changePage(pageNumber) {
		// 	console.log(pageNumber);
		// 	this.page = pageNumber;
		// },
		async fetchPosts() {
			try {
				this.isPostsLoading = true;
				const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
					params: {
						_page: this.page,
						_limit: this.limit,
					},
				});
				this.totalPage = Math.ceil(response.headers["x-total-count"] / this.limit);
				this.posts = response.data;
			} catch (e) {
				alert("Error");
			} finally {
				this.isPostsLoading = false;
			}
		},
		async loadMorePosts() {
			try {
				this.page += 1;
				const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
					params: {
						_page: this.page,
						_limit: this.limit,
					},
				});
				this.totalPage = Math.ceil(response.headers["x-total-count"] / this.limit);
				this.posts = [...this.posts, ...response.data];
			} catch (e) {
				alert("Error");
			}
		},
	},
	mounted() {
		this.fetchPosts();
		this.$refs.observer;
		// let options = {
		// 	rootMargin: "0px",
		// 	threshold: 1.0,
		// };
		// const callback = (entries, observer) => {
		// 	if (entries[0].isIntersecting && this.page < this.totalPage) {
		// 		this.loadMorePosts();
		// 	}
		// };
		// let observer = new IntersectionObserver(callback, options);
		// observer.observe(this.$refs.observer);
	},
	computed: {
		sortedPosts() {
			return [...this.posts].sort((post1, post2) => {
				return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]);
			});
		},
		sortedAndSearchedPosts() {
			return this.sortedPosts.filter((post) =>
				post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
			);
		},
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
