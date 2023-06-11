<template>
	<AppCard class="cardevent" style="cursor: pointer">
		<h5 class="card-title text-truncate">{{ title }}</h5>
		<p class="card-text text-truncate">
			{{ content }}
		</p>
		<p class="text-muted">
			{{ createDate }}
		</p>
		<template #footer>
			<div class="d-flex flex-row-reverse gap-2">
				<button
					class="btn p-0"
					@click.stop="$emit('modal')"
					style="font-size: 12px"
				>
					<i class="bi bi-box-arrow-down-right"></i>
					자세히 보기
				</button>
				<button
					class="btn p-0"
					@click.stop="$emit('preview')"
					style="font-size: 12px"
				>
					<i class="bi bi-arrow-down"></i>
					상세보기
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
defineEmits(['modal', 'preview']);
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
