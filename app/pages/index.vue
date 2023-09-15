<template>
	<div class="container">
		<h1>Rankings:</h1>
		<div v-if="!data">
			<p>loading ...</p>
		</div>
		<div v-else-if="data.length == 0">
			<p>no data yet</p>
		</div>
		<div v-else>
			<div class="user" v-for="[i, v] in  data.entries()">
				<router-link :to="`/getUser?name=${encode(v.name)}`">
					<div class="rank">
						<p>
							{{ i + 1 }}
						</p>
					</div>
					<div class="data">
						<h3>{{ v.name }}</h3>
						<p>{{ v.title }}</p>
						<p>{{ v.sumTimeString }}</p>
						<p>{{Math.floor(v.sumTime/1000) }} Total Seconds</p>
					</div>
				</router-link>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">

function encode(string: string) {
	return encodeURIComponent(string)
}

const { data } = await useAsyncData('data', async () => {
	const { ok, message, data } = await $fetch('/api/data')
	if (ok) return data
	alert(message)
	throw message
})
</script>

<style scoped lang="scss">
h1 {
	font-weight: 400;
}
.user a{
	background-color: #2e2e2e;
	display: flex;
	flex-direction: row;
	margin-bottom: 1rem; /* Increase the space between each box */
	color:black;
	text-decoration: none;
	color: white;
	font-weight: 400;
	border-radius: 1rem;
	.rank {
		border-radius: 1rem;
		display: flex;
		justify-content: center;
		align-items: center;
		font-weight: 600;
		background-color: rgb(60, 20, 124); /* Set the darker purple background */
		padding: 1rem;
		margin-right: 1rem; /* Increase the space between rank and data */
	}
	.data {
		text-decoration: none;
		margin: 0.2rem;
		h3,
		p {
			margin: 0.2rem;
		}
	}
}
</style>
