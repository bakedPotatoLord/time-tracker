<script setup lang="ts">
import Cookies from 'js-cookie'
import PinSelector from '../components/PinSelector.vue'

if (Cookies.get('id') == undefined)
	useRouter().push('/login')

const newName = ref('')
const newTitle = ref('')
const newBio = ref('')
const showLightBox = ref(false)

function getID() {
	return (
		Cookies.get('id') ??
		(() => {
			throw new Error('no id found')
		})()
	)
}
const { data: userInfo, refresh } = useAsyncData(
	'userInfo',
	async () => {
		const { data } = await $fetch('/api/getUser', {
			method: 'POST',
			body: {
				id: getID(),
			},
		})
		if (!getID()) useRouter().push('/login')
		console.log(data)
		return data
	},
)

async function handleLightboxSubmit(e: any) {
	showLightBox.value = false
	if (e.currPin == e.newPin) {
		alert('Pin cannot be the same')
	} else {
		const res = await $fetch('/api/change/pin', {
			method: 'POST',
			body: {
				pin: getID(),
				newPin: e.newPin,
			},
		})
		if (!res.ok)
			alert('something went wrong: ' + res.message)
	}
}
async function changeName() {
	const res = await $fetch('/api/change/name', {
		method: 'POST',
		body: {
			id: getID(),
			newName: newName.value,
		},
	})
	if (!res.ok) alert('something went wrong: ' + res.message)
	await refresh()
	clearInputs()
}
async function changeTitle() {
	const res = await $fetch('/api/change/title', {
		method: 'POST',
		body: {
			id: getID(),
			newTitle: newTitle.value,
		},
	})
	if (!res.ok) alert('something went wrong: ' + res.message)
	await refresh()
	clearInputs()
}
async function changeBio() {
	const res = await $fetch('/api/change/bio', {
		method: 'POST',
		body: {
			id: getID(),
			newBio: newBio.value,
		},
	})
	if (!res.ok) alert('something went wrong: ' + res.message)
	await refresh()
	clearInputs()
}
function clearInputs() {
	newName.value = ''
	newTitle.value = ''
	newBio.value = ''
}
</script>

<template>
	<div v-if="!userInfo">loading...</div>
	<div v-else class="container">
		<h1>Hello, {{ userInfo.name }}</h1>
		<h4>{{ userInfo.title }}</h4>
		<form
			class="changeSomething"
			@submit.prevent="changeName"
		>
			<label for="changeName">Name</label>
			<input
				type="text"
				id="changeName"
				v-model="newName"
				:placeholder="userInfo.name"
			/>
			<button type="submit">Change</button>
		</form>
		<form
			class="changeSomething"
			@submit.prevent="changeTitle"
		>
			<label for="changeTitle">Title</label>
			<input
				type="text"
				id="changeTitle"
				v-model="newTitle"
				:placeholder="userInfo.title"
			/>
			<button type="submit">Change</button>
		</form>
		<form
			class="changeSomething"
			@submit.prevent="changeBio"
		>
			<div class="label">
				<label for="changeBio">Bio</label>
				<button>Change</button>
			</div>
			<textarea
				v-model="newBio"
				:placeholder="userInfo.bio"
			></textarea>
		</form>
	</div>
	<button @click="showLightBox = true" id="changePin">
		Change Pin
	</button>

	
	<TimesDisplay
		:times="userInfo ? userInfo.roomTimes : []"
	/>
	<div class="footer"></div>

	<PinSelector
		:show-lightbox="showLightBox"
		@data="handleLightboxSubmit"
		@cancel="showLightBox = false"
	/>
</template>

<style scoped lang="scss">
* {
	font-family: 'Fira sans';
}
.container {
	h1 {
		margin-bottom: 0;
	}
	h4 {
		margin-top: 0.25rem;
	}
}
.changeSomething {
	background-color: #421291;
	width: min-content;
	padding: 0.3rem;
	margin-top: 1rem;
	display: flex;
	align-items: center;
	flex-direction: row;
	border-radius: 0.5rem;
	gap: 0.3rem;
	label {
		margin-left: 0;
	}
	.label {
		label {
			margin-bottom: 0.5rem;
		}
		button {
			margin-left: 0;
			padding: 0.5rem;
		}
		display: flex;
		flex-direction: column;
	}
	input,
	textarea {
		background-color: #421291;
		border-radius: 0.3rem;
		border:2px solid rgb(116, 53, 158) ;
	}
	textarea {
		width: 20rem;
		height: 5rem;
	}
}
button {
	font-weight: 600;
	background-color: #2e2e2e;
	border: 0px;
	border-radius: 0.5rem;
	background-image: linear-gradient(
		to top,
		blueviolet 50%,
		#2e2e2e 50%
	); /* Set gradient to new color */
	background-size: 100% 200%; /* Set size of gradient */
	transition: background-position 0.3s; /* Set transition duration */

	color: white;
}
button:hover {
	background-position: 0 -100%; /* Move gradient upwards */
}
#changePin {
	margin-top: 0.5rem;
	padding: 0.5rem;
}
.footer {
	margin-top: 1rem;
}
</style>
