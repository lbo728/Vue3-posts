<template>
	<div>
		<h2 @click="visiableForm = !visiableForm">게시글 생성</h2>
		<hr class="my-4" />
		<AppError v-if="error" :message="error.message" />
		<PostForm
			v-if="visiableForm"
			v-model:title="form.title"
			v-model:content="form.content"
			@submit.prevent="save"
		>
			<template #actions>
				<button type="button" class="btn btn-outline-dark" @click="goListPage">
					목록
				</button>
				<!-- <template v-if="loading">
					<button class="btn btn-primary" disabled>
						<span
							class="spinner-border spinner-border-sm"
							role="status"
							aria-hidden="true"
						></span>
						등록중...
					</button>
				</template>
				<template v-else>
					<button class="btn btn-primary">저장</button>
				</template> -->

				<button class="btn btn-primary" :disabled="loading">
					<template v-if="loading">
						<span
							class="spinner-border spinner-border-sm"
							role="status"
							aria-hidden="true"
						></span>
						등록중...
					</template>
					<template v-else>저장</template>
				</button>
			</template>
		</PostForm>
	</div>
</template>

<script setup>
// import { createPost } from '@/api/posts';
import PostForm from '@/components/posts/PostForm.vue';
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAlert } from '@/composables/alert';
import { useAxios } from '@/hooks/useAxios';

const { vAlert, vSuccess } = useAlert();

const router = useRouter();
const form = ref({
	title: null,
	content: null,
});

const { error, loading, execute } = useAxios(
	'/posts',
	{
		method: 'post',
	},
	{
		immediate: false,
		onSuccess: () => {
			router.push({ name: 'PostList' });
			vSuccess('등록이 완료되었습니다');
		},
		onError: err => {
			vAlert(err.message);
		},
	},
);
const save = async () => {
	execute({ ...form.value, createAt: Date.now() });
};
// const save = async () => {
// 	try {
// 		loading.value = true;
// 		await createPost({
// 			...form.value,
// 			createdAt: Date.now(),
// 		});

// 	} catch (err) {
// 		vAlert(err.message);
// 		error.value = err;
// 	} finally {
// 		loading.value = false;
// 	}
// };
const goListPage = () => router.push({ name: 'PostList' });
const visiableForm = ref(true);
</script>

<style lang="scss" scoped></style>
