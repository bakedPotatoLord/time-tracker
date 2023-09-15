<script setup lang="ts">
interface PinData {
	currPin: number
	newPin: number
}
defineProps({
	showLightbox: {
		type: Boolean,
		required: true,
	},
})
const emits = defineEmits<{
	(event: 'data', data: PinData): void
	(event: 'cancel'): void
}>()
const currentPin = ref<number | null>(null)
const newPin = ref<number | null>(null)
function handleSubmit(e: Event) {
	e.preventDefault()
	emits('data', <PinData>{
		currPin: currentPin.value,
		newPin: newPin.value,
	})
	clearFields()
}
function handleCancel() {
	emits('cancel')
	clearFields()
}
function clearFields() {
	currentPin.value = null
	newPin.value = null
}
</script>

<template>
	<div class="lightbox" v-if="showLightbox">
		<div class="lightbox-content">
			<h2>Pin Change</h2>
			<form class="form" @submit.prevent="handleSubmit">
				<label for="currPin">Current pin</label>
				<input
					type="number"
					id="currPin"
					v-model.number="currentPin"
					required
				/>
				<label for="newpin">New Pin</label>
				<input
					type="password"
					id="newpin"
					v-model.number="newPin"
					name="pin"
					required
				/>
				<div class="escapeOptions">
					<button type="submit">Submit</button>
					<button @click.prevent="handleCancel">Cancel</button>
				</div>
			</form>
		</div>
	</div>
</template>

<style scoped lang="scss">
.lightbox {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background-color: rgba(0, 0, 0, 0.8);
	display: flex;
	justify-content: center;
	align-items: center;
	input {
		display: block;
		margin-bottom: 10px;
	}
	.lightbox-content {
		background-color: #2e2e2e;
		padding: 20px;
		form{
			input,
			button{
				background-color:rgb(92, 37, 180);
				border:2px solid #2e2e2e;
				border-radius:0.5rem;
				padding-left: 0.5rem;
				padding-right: 0.5rem;
				margin-bottom: 0.5rem;
				color:white;
			}
			button{
				font-family: 'Montserrat', sans-serif;
				padding: 0.4rem;
			}
		}
	}
}
.escapeOptions {
	display: flex;
	justify-content: space-between;
}
</style>
