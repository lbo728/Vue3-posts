<template>
	<div class="text-center py-5">
		<img src="@/assets/404.svg" width="640" />
		<p class="h3 mt-5 text-secondary" style="font-weight: 400">
			요청하신 페이지를 찾을 수 없습니다!
		</p>

		<div class="mt-5">
			<RouterLink to="/">
				<button class="btn btn-primary">Home</button>
			</RouterLink>
		</div>
	</div>
</template>

<script setup></script>

<style lang="scss" scoped></style>
