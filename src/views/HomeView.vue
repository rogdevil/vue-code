<script setup lang="ts">
import { watchEffect, ref } from "vue";
import type { IUser } from "@/interfaces";
import TableRow from "@/components/TableRow.vue";

enum EUser {
	slno = "slno",
	name = "name",
	dob = "dob",
	email = "email",
	location = "location",
	phone = "phone",
	picture = "picture",
}

const userData = ref<IUser[]>();

const sort = ref({
	type: "",
	isDesc: true,
});

const API_URL = "https://randomuser.me/api/?results=50";

const flattenData = (data: any) => {
	const tempAry: IUser[] = [];

	for (let i = 0; i < 50; i++) {
		let obj: IUser = {} as IUser;
		obj.name = `${data[i].name.title}. ${data[i].name.first} ${data[i].name.last}`;
		obj.slno = `${data[i].id.name}-${data[i].id.value}`;
		obj.dob = `${data[i].dob.date.split("T")[0].split("-").join("/")}`;
		obj.email = data[i].email;
		obj.location = `#${data[i].location.street.number} ${data[i].location.street.name}, ${data[i].location.city}, ${data[i].location.state}, ${data[i].location.country}-${data[i].location.postcode}`;
		obj.phone = data[i].phone;
		obj.picture = data[i].picture.thumbnail;
		tempAry.push(obj);
	}

	userData.value = tempAry;
};

watchEffect(async () => {
	const res = await (await fetch(API_URL)).json();
	flattenData(res.results);
});

const filterData = (key: EUser) => {
	if (userData && userData.value) {
		sort.value.type = key;
		let firstKey: number | string | Date, secondKey: number | string | Date;
		if (sort.value.isDesc) {
			sort.value.isDesc = false;

			userData.value.sort((a, b) => {
				if (key === "dob") {
					firstKey = new Date(a[`${key}`]);
					secondKey = new Date(b[`${key}`]);
				} else if (key === "phone") {
					firstKey = parseInt(a[`${key}`]);
					secondKey = parseInt(b[`${key}`]);
				} else {
					firstKey = a[`${key}`];
					secondKey = b[`${key}`];
				}
				return firstKey > secondKey ? 1 : secondKey > firstKey ? -1 : 0;
			});
		} else {
			sort.value.isDesc = true;

			userData.value.sort((a, b) => {
				if (key === "dob") {
					firstKey = new Date(a[`${key}`]);
					console.log(firstKey, a[`${key}`]);

					secondKey = new Date(b[`${key}`]);
					console.log(secondKey, b[`${key}`]);
					console.log(firstKey < secondKey);
				} else if (key === "phone") {
					firstKey = parseInt(a[`${key}`]);
					secondKey = parseInt(b[`${key}`]);
				} else {
					firstKey = a[`${key}`];
					secondKey = b[`${key}`];
				}
				return firstKey < secondKey ? 1 : secondKey < firstKey ? -1 : 0;
			});
		}
	}
};
</script>

<template>
	<main>
		<table v-if="userData">
			<thead>
				<TableRow>
					<th :onClick="() => filterData(EUser.name)">
						Name
						<span v-if="sort.type === 'name' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'name' && sort.isDesc"
							>⬇</span
						>
					</th>
					<th :onClick="() => filterData(EUser.slno)">
						Id
						<span v-if="sort.type === 'slno' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'slno' && sort.isDesc"
							>⬇</span
						>
					</th>
					<th :onClick="() => filterData(EUser.dob)">
						Date of Birth
						<span v-if="sort.type === 'dob' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'dob' && sort.isDesc">⬇</span>
					</th>
					<th :onClick="() => filterData(EUser.email)">
						Email
						<span v-if="sort.type === 'email' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'email' && sort.isDesc"
							>⬇</span
						>
					</th>
					<th :onClick="() => filterData(EUser.location)">
						Address
						<span v-if="sort.type === 'location' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'location' && sort.isDesc"
							>⬇</span
						>
					</th>
					<th :onClick="() => filterData(EUser.phone)">
						Phone
						<span v-if="sort.type === 'phone' && !sort.isDesc"
							>⬆</span
						>
						<span v-if="sort.type === 'phone' && sort.isDesc"
							>⬇</span
						>
					</th>
					<th>Image</th>
				</TableRow>
			</thead>
			<tbody>
				<TableRow
					v-for="user in userData"
					:key="user.email"
					:user="user"
				></TableRow>
			</tbody>
		</table>
	</main>
</template>
<style>
th {
	cursor: pointer;
	min-width: 200px;
	text-align: center;
	background: #000;
	color: #fff;
}
tr {
	min-width: 200px;
	text-align: center;
}
</style>
