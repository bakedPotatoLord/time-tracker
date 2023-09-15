<template>
	<div class="container">
		<div class="topStuff">

			<Top />
			<TopDivider/>
		</div>
		<div class="router-container">
			<NuxtPage />
		</div>
	</div>
</template>

<style scoped>
.topStuff {
	position:fixed;
	top:0;
	left:0;
	width:100%;
}
.router-container {
	margin-top: 150px;
	margin-left: 1rem;
	margin-right: 1rem;
}
</style>

<style src="@/assets/css/style.css"></style>
