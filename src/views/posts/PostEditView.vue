<template>
	<AppLoading v-if="loading" />

	<!-- <AppError v-else-if="error" :message="error.message" /> -->
	<div v-else>
		<h2>게시글 수정</h2>
		<AppError v-if="editError" :message="editError.message" />
		<hr class="my-4" />
		<PostForm
			v-model:title="form.title"
			v-model:content="form.content"
			@submit.prevent="edit"
		>
			<template #actions>
				<button
					type="button"
					class="btn btn-outline-danger"
					@click="goDetailPage"
				>
					취소
				</button>
				<button class="btn btn-primary" :disabled="editLoading">
					<template v-if="editLoading">
						<span
							class="spinner-border spinner-border-sm"
							role="status"
							aria-hidden="true"
						></span>
						등록중...
					</template>
					<template v-else> 수정 </template>
				</button>
			</template>
		</PostForm>
		<!-- <AppAlert :show="showAlert" :message="alertMessage" :type="alertType" /> -->
	</div>
</template>

<script setup>
// import { ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
// import { getPostById, updatePost } from '@/api/posts';
import { useAlert } from '@/composables/alert';
import PostForm from '@/components/posts/PostForm.vue';
import { useAxios } from '@/hooks/useAxios';

const { vAlert, vSuccess } = useAlert();

const route = useRoute();
const router = useRouter();
const id = route.params.id;

const { data: form, loading } = useAxios(`/posts/${id}`);

const {
	error: editError,
	loading: editLoading,
	execute,
} = useAxios(
	`/posts/${id}`,
	{ method: 'patch' },
	{
		immediate: false,
		onSuccess: () => {
			vSuccess('수정이 완료되었습니다!');
			router.push({ name: 'PostDetail', params: { id } });
		},
		onError: err => {
			vAlert(err.message);
		},
	},
);

const edit = () => {
	execute({
		...form.value,
	});
};

// const editError = ref(null);
// const editLoading = ref(false);

// const edit = async () => {
// 	try {
// 		editLoading.value = true;
// 		await updatePost(id, { ...form.value });

// 	} catch (err) {
// 		vAlert(err.message);
// 		editError.value = err;
// 	} finally {
// 		editLoading.value = false;
// 	}
// };

const goDetailPage = () =>
	router.push({
		name: 'PostDetail',
		params: { id },
	});
</script>

<style lang="scss" scoped></style>
