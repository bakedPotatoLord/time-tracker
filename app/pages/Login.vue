<script setup lang="ts">
const userPin = ref<string | null>(null)

async function login(e: Event) {
	e.preventDefault()
	$fetch('/api/login', {
		method: 'POST',
		body: {
			pin: userPin.value,
		},
	}).then(res => {
		if (res.ok) {
			useRouter().push('/MyAcct')
		} else {
			alert('something went wrong: ' + res.message)
		}
	})
}
</script>


<template>
	<h1>Login</h1>
	<br />
	<form @submit.prevent = "login">
		<label for="pin">Pin</label><br>
		<input
			type="number"
			name="pin"
			placeholder="0000"
			v-model="userPin"
		/>
		<br />
		<input type = "submit" value="Login">
	</form>
	<p>
		
		<router-link to="/createAcct">Create an account instead</router-link>
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
