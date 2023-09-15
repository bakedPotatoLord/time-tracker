<script setup lang="ts">
const name = ref('')
const title = ref('')
const pin = ref(null)
const pin2 = ref(null)

async function submitForm(e: Event) {
	e.preventDefault()
	if (pin.value != pin2.value) {
		alert('pin does not match')
		return
	}
	$fetch('/api/create', {
		method: 'POST',
		body: {
			pin: Number(pin.value),
			name: name.value,
			title: title.value,
		},
	}).then(res => {
		if (res.ok) {
			alert('account created')
		} else {
			alert('something went wrong: ' + res.message)
		}
	})
}
</script>

<template>
	<h1>Account Creation</h1>
	<form type="POST" v-on:submit="e => submitForm(e)">
		<label for="name">
			Name (preferably your real one)
		</label>
		<br />
		<input
			type="text"
			name="name"
			v-model="name"
			required
		/>
		<br />
		<label for="title">Title (your job on the team)</label>
		<br />
		<input
			type="text"
			name="title"
			v-model="title"
			required
		/>
		<br />
		<label for="pin">Pin</label>
		<br />
		<input
			type="number"
			name="pin"
			min="0"
			max="9999"
			v-model="pin"
			required
		/>
		<br />
		<label for="pin2">Retype Pin</label>
		<br />
		<input
			type="number"
			name="pin2"
			min="0"
			max="9999"
			v-model="pin2"
			required
		/>
		<br />
		<input type="submit" value="Submit" />
	</form>
	<p>
		<router-link to="/login">Login Instead</router-link>
	</p>
</template>

<style scoped lang="scss">
form{
	
	input{
		background-color:rgb(92, 37, 180);
		border:2px solid #2e2e2e;
		border-radius:0.5rem;
		padding-left: 0.5rem;
		padding-right: 0.5rem;
		margin-bottom: 0.5rem;
		color:white;
	}
	input[type="submit"]{
		font-family: 'Montserrat', sans-serif;
	}
}
p{
	a{
		color: white;
		text-decoration: underline;
	}
}
</style>
