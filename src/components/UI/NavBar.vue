<template>
	<div class="navbar">
		<div class="" @click="$router.push('/')">Vue3</div>
		<div class="navbar__btns">
			<MyButton @click="$router.push('/posts')">Posts</MyButton>
			<MyButton @click="$router.push('/store')">Posts store</MyButton>
			<MyButton style="margin-left: 20px" @click="$router.push('/compApi')">Posts compApi</MyButton>
			<MyButton style="margin-left: 20px" @click="$router.push('/about')">About us</MyButton>
		</div>
	</div>
</template>

<script>
export default {};
</script>

<style scoped>
.navbar {
	height: 50px;
	background: lightgray;
	box-shadow: 2px 2px 4px gray;
	display: flex;
	align-items: center;
	padding: 0 15px;
}
.navbar__btns {
	margin-left: auto;
}
</style>
