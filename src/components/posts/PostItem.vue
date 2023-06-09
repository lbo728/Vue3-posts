<template>
	<AppCard class="cardevent" style="cursor: pointer">
		<h5 class="card-title">{{ title }}</h5>
		<p class="card-text">
			{{ content }}
		</p>
		<p class="text-muted">
			{{ createDate }}
		</p>
		<template #footer>
			<div class="d-flex flex-row-reverse">
				<button
					class="btn p-0"
					@click.stop="$emit('modal')"
					style="font-size: 12px"
				>
					<!-- <i class="bi bi-emoji-sunglasses-fill"></i> -->
					자세히 보기
					<i class="bi bi-arrow-right"></i>
				</button>
			</div>
		</template>
	</AppCard>
</template>

<script setup>
import { computed } from 'vue';
import { inject } from 'vue';

const props = defineProps({
	title: {
		type: String,
		required: true,
	},
	content: {
		type: String,
	},
	createAt: {
		type: [String, Date, Number],
	},
});
defineEmits(['modal']);
const dayjs = inject('dayjs');
const createDate = computed(() =>
	dayjs(props.createAt).format('YYYY.MM.DD HH:mm:ss'),
);
</script>

<style scoped>
.cardevent:hover {
	border: 1px solid blue;
}
</style>
