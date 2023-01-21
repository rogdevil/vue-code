<script
	setup
	lang="ts"
>
import type { PropType } from "vue";

const {
	data,
	isImage = false,
} =
	defineProps(
		{
			data: {
				type: String,
				required:
					true,
			},
			isImage:
				Boolean,
		}
	);
</script>
<template>
	<td
		:width="
			200
		"
	>
		<img
			v-if="
				isImage
			"
			:src="(data as string)"
			alt="data"
		/>
		<p
			v-else
		>
			{{
				data
			}}
		</p>
	</td>
</template>
