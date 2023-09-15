<script lang="ts" setup>
const numTimesShown = ref(15)

const sortType = ref('td')

function topTimes() {
	if (sortType.value == 'da')
		return props.times
			.map(time => [...time, time[1] - time[0]])
			.sort((a, b) => b[0] - a[0])
			.slice(0, numTimesShown.value)
			.map(time => [
				new Date(time[0]).toLocaleDateString(),
				`${(time[2] / (1000 * 60 * 60)).toFixed(2)} hours`,
			])
	else if (sortType.value == 'dd')
		return props.times
			.map(time => [...time, time[1] - time[0]])
			.sort((a, b) => a[0] - b[0])
			.slice(0, numTimesShown.value)
			.map(time => [
				new Date(time[0]).toLocaleDateString(),
				`${(time[2] / (1000 * 60 * 60)).toFixed(2)} hours`,
			])
	else if (sortType.value == 'ta')
		return props.times
			.map(time => [...time, time[1] - time[0]])
			.sort((a, b) => a[2] - b[2])
			.slice(0, numTimesShown.value)
			.map(time => [
				new Date(time[0]).toLocaleDateString(),
				`${(time[2] / (1000 * 60 * 60)).toFixed(2)} hours`,
			])
	else if (sortType.value == 'td')
		return props.times
			.map(time => [...time, time[1] - time[0]])
			.sort((a, b) => b[2] - a[2])
			.slice(0, numTimesShown.value)
			.map(time => [
				new Date(time[0]).toLocaleDateString(),
				`${(time[2] / (1000 * 60 * 60)).toFixed(2)} hours`,
			])
}

const props = defineProps({
	times: {
		type: Array<number[]>,
		required: true,
	},
})
</script>

<template>
	<div class="head">
		<div class="left">
			<h3>Top Times</h3>
		</div>
		<div class="right">
			<p>Sort by:</p>
			<select v-model="sortType" name="sort">
				<option value="td">Time (descending)</option>
				<option value="dd">Date (descending)</option>
				<option value="ta">Time (ascending)</option>
				<option value="da">Date (ascending)</option>
			</select>
		</div>
	</div>
	<br />
	<div class="times">
		<div class="time" v-for="time in topTimes()">
			<h3>{{ time[0] }}</h3>
			<h5>{{ time[1] }}</h5>
		</div>
	</div>
	<button @click="numTimesShown = Infinity">
		Show All
	</button>
</template>

<style scoped lang="scss">
.head {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-right: 1rem;
	margin-left: 1rem;
	.right {
		display: flex;
		align-items: center;
		flex-direction: row;
		gap: 0.5rem;
		select {
			background-color: #2e2e2e;
			color: white;
			font-size: 1.1rem;
			option:hover {
				background-color: #421291;
			}
		}
	}
}
.times {
	font-family: 'Fira sans';
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	justify-content: center;
	.time {
		text-align: center;
		background-color: #421291;
		margin: 0.5rem;
		padding: 0.5rem;
		border-radius: 1rem;
		box-shadow: 0 0 0 0 rgba(0, 0, 0, 0.2);
		h3 {
			margin-top: 0;
			margin-bottom: 0.2rem;
		}
		h5 {
			margin: 0;
		}
		&:hover {
			box-shadow: 0 0 10px 3px rgba(138, 43, 266, 1);
		}
	}
}
button {
	font-weight: 600;
	background-color: #421291;
	color: white;
	border: 0px;
	border-radius: 0.5rem;
	background-image: linear-gradient(
		to top,
		rgb(59, 20, 95) 50%,
		#421291 50%
	); /* Set gradient to new color */
	background-size: 100% 200%; /* Set size of gradient */
	transition: background-position 0.3s; /* Set transition duration */
	padding: 0.2rem;
	&:hover {
		background-position: 0 -100%; /* Move gradient upwards */
	}
}
</style>
