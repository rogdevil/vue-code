<script
	setup
	lang="ts"
>
import type { IUser } from "@/interfaces";
import type { PropType } from "vue";
import TableData from "./TableData.vue";

const {
	user,
} =
	defineProps(
		{
			user: {
				type: Object as PropType<IUser>,
				required:
					false,
			},
		}
	);
</script>
<template>
	<tr>
		<slot
			:data="
				user
			"
		>
		</slot>
		<TableData
			v-if="
				user
			"
			v-for="(
				item,
				key,
				index
			) in user"
			:data="
				item
			"
			:key="
				user.email
			"
			:is-image="
				key ===
				'picture'
			"
		></TableData>
	</tr>
</template>
